---
title: FSAPT analysis
layout: default
parent: "How to use RINRUS"
nav_order: 6
---

# F(/I)-SAPT analysis and model fragment re-ranking

These scripts and instructions assume that the user has created a probe/arpeggio/distance maximal model and then run an F(/I)-SAPT calculation on it.
Generally fragment A should be the seed and fragment B should be the rest of the model. 
If the seed is covalently bound to other stuff it's up to the user to decide whether to put that stuff into fragment A as well or do I-SAPT. 

**RINRUS I-SAPT functionality and support is currently limited.** Significant testing on how to design and automate sensible choices of fragments A/B/C is in progress in the DeYonker group.
Analysis scripts work for I-SAPT outputs but inputs must be made by hand and the usefulness of the results depends entirely on the user's definitions of A/B/C.

**Run scripts within the `fsapt/` output directory**

## Use `define_fA_fB.py` to define the functional groups of the fragments

```bash
# Usage of define_fA_fB
python $HOME/git/RINRUS/bin/FSAPT/define_fA_fB.py -pdb ../model.pdb -inp ../input.dat -s A:128

# Arguments for define_fA_fB
-pdb PDB    pdb of model F-SAPT calc was done on
-inp INPUT  F-SAPT calculation input file (default: ../input.dat)
-s SEED     seed species as ch:ID(,ch:ID,..)
```

This generates the files `fA.dat`, `fB.dat`, `frag_atom_check.dat` and `fdict.pkl`
* `fA.dat` and `fB.dat` contain the lists of functional groups/atom numbers used by the psi4 fsapt.py processing script. 
    - Fragment A is partitioned simply by residue IDs. 
    - Fragment B is partitioned into side chains, main chains, solvent molecules, small molecules, etc. If CAs of non-glycine residues are in the model without their side chains (i.e. only present to cap/connect main chains), all of these CAs and their hydrogens are grouped together as "capping atoms". 
    - It may be worthwhile to edit `fA.dat` for a more useful partitioning.
* `frag_atom_check.dat` contains a table of info for each atom including the atom number from both the reordered F/I-SAPT input file and original pdb. Makes it easier to check/modify `fA.dat`/`fB.dat` (and make `fX.dat` if needed)
* `fdict.pkl` stores the fragment-grouped atom names for reuse in `fisapt_analysis.py`

**Specified pdb and input file must have exact same geometry.** Be careful not to mix up optimized structures and original template pdbs etc. This script gets the atom order from the F/I-SAPT input file and uses the atom coords to get the matching name/res info from the model pdb. This avoids the mislabelling that used to occur with the old scripts when the order of the atoms within each fragment did not match their order within the model pdb (easy to do this accidentally when manually making input files). 

Seed should be the seed selected for RINRUS generally, not necessarily the exact contents of fragment A in the F/I-SAPT calculation. This is just used to help sort the atoms for the final `res_atoms_fsapt.dat` file. 

<details>
    <summary>If doing I-SAPT analysis, fX.dat must be made by hand. Click for more info.</summary>

> `fX.dat` specifies the atom numbers of the atoms in fragments A and B that are bound to the linker fragment C. The file contents should look something like:
> ```
> A 1
> B 20
> ```
> To get the correct atom numbers, you can visualise the model pdb to find the pdb-ordered atom numbers and then get their input-ordered atom numbers from `frag_atom_check.dat`.
</details>

## Use `fisapt_analysis.py` to do the fsapt processing, sort the FGs by |Eint|, and create `res_atoms_fsapt.dat`
```bash
# Default usage of fisapt_analysis
python $HOME/git/RINRUS/bin/FSAPT/fisapt_analysis.py

# Optional arguments for fisapt_analysis
-path PATH     path to the psi4 fsapt.py processing script (default: locates from psi4 install)
-save PATH     where to save sorted Eint summary file and res_atoms_fsapt.dat (default: ../)
-rank SEED-FG  rank FGs by |Eint| with whole seed ('All', is default) or just part of seed (any group specified in fA.dat)
```

This runs the psi4 `fsapt.py` processing script to partition the energies by the groups defined in `fA.dat`/`fB.dat`, which creates `fsapt.dat`.
The interaction energies (Links 50-50, total values) between each FG in the model and each part of the seed are extracted from `fsapt.dat` and summarised in `FG-SAPT-ranked.dat`. 
By default the model FGs are ranked by their |Eint| with the whole seed, but any specific part of the seed defined in `fA.dat` can be used. 
This ranking is used along with the atom names/grouping stored in `fdict.pkl` to create `res_atoms_fsapt.dat`.

## Create new set of sequential models from `res_atoms_fsapt.dat`

[Instructions for model trimming and capping](Trimming.html)