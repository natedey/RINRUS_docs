---
title: 1. Active site selection
layout: default
parent: "How to use RINRUS"
nav_order: 3
---

# Selecting the active site/generating the active site RIN

***Defining the "seed" is one of the most important steps of the QM-cluster model building process.*** What should you select as the seed? Typically, the seed will be the substrate(s) (or ligand in biochemical terms) participating in the chemical reaction. Any amino acid residues, co-factors, or fragments which participate in the active site catalytic breaking and forming of chemical bonds may also need to be included as part of the seed, but this will generate much larger models compare to only using the substrate.

The seed is specified as a comma-separated list of colon-separated Chain:ResidueID pairs. In the example of 3BWM, we select the seed as A:300 (Mg2+), A:301 (SAM) and A:302 (catechol). 

If the PDB you're using does not have chain identifiers, you will need to specify ":XXX" where XXX is the residue ID number in this step and beyond. Our current defaults are wonky in these cases and need to be improved. If the protein is multimeric, use the chain of your choice for seed fragments. Note that some multimeric x-ray crystal structures may not necessarily have equivalent active sites!

The active site RIN can be automatically determined from the seed fragments based on probe contacts, arpeggio contacts or distance. For all selection metrics the key output is a file, usually called `res_atoms.dat` or similar, containing a list of the identified active site residues ranked by the chosen metric and the atoms identified by the selection procedure. This file will be used as the input for the trimming procedure in part 3.

## A. Using Probe contact count ranking

First run `probe` on the (modified) PDB file to generate a *.probe file of all contacts in the enzyme
``` bash
# Example usage of probe:
$HOME/git/RINRUS/bin/probe -unformated -MC -self "all" 3bwm_h_modify.pdb > 3bwm_h_modify.probe 
```

Then generate the active site RIN from the probe contacts with `probe2rins.py`
``` bash
# Example usage of probe2rins:
python3 $HOME/git/RINRUS/bin/probe2rins.py -f 3bwm_h_modify.probe -s A:300,A:301,A:302

# All arguments for probe2rins:
-f FILE   probe contacts file
-s SEED   seed fragment(s) (e.g. A:300,A:301,A:302)
```

This produces `freq_per_res.dat`, `rin_list.dat`, `res_atoms.dat`, and `*.sif`.

**Use `res_atoms.dat` as the input for the trimming procedure in part 3.**

**NOTE:** Remember to replace the metal atom in the PDB before continuing/use the unmodified PDB for the remaining steps if it was replaced with O in the preprocessing.

## B. Using Arpeggio contact count or contact type ranking

*Make sure openbabel libraries are available to properly use RINRUS with arpeggio.*

First, run `arpeggio` to generate the contact file (you need to make sure config.py is in the same directory as arpeggio.py)
````bash
# Example usage of arpeggio
python3 ~/git/RINRUS/bin/arpeggio/arpeggio.py 3bwm_h.pdb
````  

Then generate the active site RIN from arpeggio contacts using `arpeggio2rins.py`
````bash
# Example usage of arpeggio2rins
python3 ~/git/RINRUS/bin/arpeggio2rins.py -f 3bwm_h.contacts -s A:300,A:301,A:302

# All arguments for arpeggio2rins:
-f FILE   arpeggio contacts file
-s SEED   seed fragment(s) (A:300,A:301,A:302)
````
This produces the files `contact_counts.dat`, `contype_counts.dat` and, `node_info.dat`. Both `contact_counts.dat` and `contype_counts.dat` have the same format as `res_atoms.dat`.

**Use `contact_counts.dat` (residues ranked by number of contacts) or `contype_counts.dat` (residues ranked by number of interaction types) as the input for the trimming procedure in part 3.**

## C. Using distance ranking

There are two key options that determine how RINRUS calculates the distance between residues and the seed for distance-based selection and ranking. 
* Distance type: distance can be calculated to the seed's centre of mass or average Cartesian coordinates, or the closest seed atom.
* Hydrogens: distances can be calculated using all atoms or only heavy atoms (all hydrogens are ignored). This applies to both the residue-seed distance calculation and calculation of the seed COM/average centre.

Use `dist_rank.py` to select all fragments with any atoms within a cutoff radius of the seed. A limited set of seed atoms can be selected for the distance calculations by using the '-satom' flag instead of '-s'.
```bash
# Example usage of dist_rank selecting residues within 5A of the full seed COM
python3 ~/git/RINRUS/bin/dist_rank.py -pdb 3bwm_h.pdb -s A:300,A:301,A:302 -max 5 -type mass

# All arguments for dist_rank
-type TYPE    how to calculate distance from seed ('closest' or 'avg' or 'mass')
-noH          ignore hydrogen atoms (true if flag present)
-max CUTOFF   cut off distance in Å (default: 5)
-s SEED       seed fragment(s) (e.g. A:300,A:301,A:302)
-satom ATOMS  seed atoms (e.g. A:301:C8,A:301:N9,A:302:C1,A:302:N1)
```

This produces a file `all_atoms_[type]_[max].dat` listing all atoms within the cutoff distance, which are then grouped by residue IDs to give `sorted_residues_[type]_[max].dat` and `res_atoms_by_residue.dat`. The atom list is also grouped by functional groups (matching the SC/MC partitioning used for F-SAPT) to give `sorted_FGs_[type]_[max].dat` and `res_atoms_by_FG.dat`.

**Use `res_atoms_by_residue.dat` or `res_atoms_by_FG.dat` as the input for the trimming procedure in part 3.**

## D. Manual selection and ranking

You can generate your own `res_atoms.dat` file using an existing `res_atoms.dat` file as a template or from scratch.
* The first two columns should list the chain and residue ID of a given residue.
* The third column is where the ranking value would go. This isn't actually used by the trimming script so the value doesn't matter as long as something is there.
* The rest of the line should be the selected atom(s). 

The residues should be listed in the order you want them to be added to the model.

Example format:
```
A    300      554      O   
A    40       491      CB   CE   CG   HB2  HE1  HE3  HG3  O    SD  
A    141      478      CB   CG   HB3  O    OD1  OD2 
A    143      378      CD1  CD2  CE2  CE3  CG   CH2  CZ2  CZ3  H    NE1 
A    91       335      CB   CG2  H    HB   HG21 HG22 N   
A    170      304      CG   HD21 ND2  OD1
```

**Use your new `res_atoms.dat` as the input for the trimming procedure in part 3.**

**Next:** [Model trimming and capping](Trimming.html)