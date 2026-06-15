---
title: rinrus_trim2_pdb
layout: default
parent: "Individual functions"
---

# rinrus_trim2_pdb.py

This script uses the seed and atom selection in `res_atoms.dat` to trim the starting PDB structure into QM-cluster model(s). It also determines which atoms from each residue need to be frozen in the computations and flags them in the output structure. 

## Help message
```
usage: rinrus_trim2_pdb.py [-h] [-pdb R_PDB] [-s SEED] [-ra R_ATOM] [-ncres NCRES] [-unfrozen UFREE] [-model METHOD]
                           [-approx_model_size_limit NATOMS] [-mustadd MUSTADD]

Trim large PDB file according to res_atoms.dat, write trimmed pdb in working directory

options:
  -h, --help            show this help message and exit
  -pdb R_PDB            Protonated pdbfile
  -s SEED, -seed SEED   Seed specified in form "Chain:Resid,Chain:Resid"
  -ra R_ATOM            res_atoms file containing atom info for each residue
  -ncres NCRES          Noncanonical residue information
  -unfrozen UFREE       Atoms/residues to avoid constraining. Ch:ID to unfreeze all, or ch:ID:CA or ch:ID:CB
  -model METHOD         Generate one or all trimmed models, if "7" is given, then will generate the 7th model, "max" for only
                        maximal model
  -approx_model_size_limit NATOMS
                        Pick model by number of atoms instead of number of fragments (overrides model argument)
  -mustadd MUSTADD      Necessary non-seed fragments ([S]ide chain, [N]-term, [C]-term) e.g. "A:7:S+C,A:8:N"
```

## Inputs

- `-pdb`: Processed PDB to be trimmed (required)
  - Given as filename
  - Example: `-pdb 2cht_h.pdb`
- `-s`: Seed
  - Given as ch:ID pairs
  - Examples: `-s A:203` or `-seed A:301,A:302`
- `-ra`: res_atoms.dat file to read atom selection from:
  - Given as filename, default value: res_atoms.dat
  - Example: `-ra res_atoms_types.dat`
- `-ncres`: File containing information about non-canonical residues
  - Given as filename
  - File should contain lines listing the 3 letter residue code and names of all atoms in the non-canonical residue. E.g. for the methylated lysine in [PDB:1O9S](https://www.rcsb.org/structure/1O9S) with residue name MLZ:
    ```
    MLZ  N CA C O CB CG CD CE NZ CM HZ1 HZ2 H HA HB2 HB3 HG2 HG3 HD2 HD3 HE2 HE3 HCM1 HCM2 HCM3
    ```
  - Example: `-ncres ncres_info.dat`
- `-unfrozen`: Atoms to avoid freezing
  - Given as list of ch:ID[:atom] identifiers
  - ch:ID removes any and all constraints on the residue, ch:ID:CB unfreezes just CB (leaving CA frozen), and ch:ID:CA unfreezes just CA (CB may still be frozen depending on the residue)
  - Example: `-unfrozen A:7:CB,A:8:CA,A:9`
- `-model`: Model(s) to make
  - Given as "max"/"maximal"/"all"/[integer], default value: "all"
  - If integer value given, script adds seed fragments, then mustadd fragments, then lines of res_atoms until that number is reached. Maximal model is always created for comparison.
  - Examples: `-model max` or `-model 10`
- `-approx_model_size_limit`: Approximate desired model size (overrides -model argument)
  - Given as number of atoms
  - Makes the first model from the sequence whose size pre-capping is >=90% of X (or the maximal model if that threshold is not reached)
  - Example: `-approx_model_size_limit 200`
- `-mustadd`: Fragments that need to be added to all models but aren't in the seed (e.g. residues that coordinate a metal ion)
  - Given as ch:ID[:part] identifiers. Parts are [S]ide chain, [N]-terminus, [C]-terminus; multiple parts given like S+C, C+N, etc. If no part(s) specified, whole residue added.
  - Example: `-mustadd A:7:C,A:8,A:9:S+N`


## Outputs

For each trimmed model:
- `res_N.pdb`: trimmed model in PDB format with constraint flags
- `res_N_atom_info.dat`: list of atoms added to the model
- `res_N_atom_info.dat`: list of atoms frozen in the model

If all models have been made:
- `seq_model_contents.dat`: file showing which functional groups are in each model and identifying duplicate models



## Quick pseudocode explanation of how the script works for each model

_This is kinda rough because I wrote it for internal group use, but included here in case it's helpful for others -DAW_

```
- read args
- process non-canonical res info
- process unfrozen atoms
- read pdb
- process seed
- read in res_atoms, remove comment lines
- loop through pdb creating pdb_res_name and pdb_res_atom dictionaries - pdb_res_atom contains all atoms in pdb for given resid
  - (if making models other than the maximal model, pdb_res_atom gets updated after making the maximal model so that smaller models can only be made from the contents of the maximal model)
- get max and min size of models based on no. seed fragments, no. must_add groups, length of res_atoms
- run trimming function on model(s)
  - for seed lines of res_atoms, add atoms to res_part_list dictionary - dict is like {(A,203): ['C1','O1','C2','O2']} etc for each resid
  - add must_add groups to res_part_list
  - add lines of res_atoms to res_part_list until size of res_part_list is desired model size
  - (if making a model other than the first or last, add contents of previous model to res_part_list too to ensure that each model builds on the last one)
  - check atoms in each entry of res_part_list
    - if seed or water, add all atoms from that group in original pdb to model atom info. frozen atom list is empty.
    - otherwise:
      - run check_sc function
        - if res is proline, include all atoms from standard rinrus res atom dict
        - if res is in standard res dict and has SC atoms, add all SC atoms
        - if res is in non-canonical res dict and has SC atoms, add all SC atoms
        - if res is not recognized, use original atom list from res_atoms and print warning
      - if any side chain atoms, make sure CA/HA in there and freeze CA
      - if no SC atoms, complete relevant part of MC and make sure CA/HA in there
  - complete peptide bonds
    - checks that group is not seed and not water and res+1/res-1 are not water either
    - if N and H in there, make sure C,O,CA,HA of previous residue are in there
    - if C and O in there, make sure N,H,CA,HA of next residue are in there
      - if next residue is proline, make sure to add whole proline and add N,H,CA,HA of res+2 to complete that peptide bond too
  - check for disconnected adjacent CAs, if found then add peptide bond between
  - if residue has only CA/HA or is alanine side chain
    - add peptide bond on both sides up to CAs
    - check if this has made more disconnected adjacent CAs, if yes then connect
  - check for any partial prolines (from adding peptide bonds to connect CA-CA or anchor CAs/ala SC)
    - if found, add SC and MC if necessary.
    - again check if this has made more disconnected adjacent CAs and connect if needed
    - repeat this check until no new groups added and everything completed/connected as they should be
  - check frozen info
    - make sure all CAs are frozen
    - freeze CB for ['ARG','LYS','GLU','GLN','MET','TRP','TYR','PHE']
    - unfreeze any atoms specified to be unfrozen
  - write final atom lists to res_N_atom_info.dat
  - write final frozen lists to res_N_froz_info.dat
  - write res_N.pdb
```
