---
title: Group notes/to-do
layout: home
nav_order: 6
---

{: .warning }
> Page for group use and not regularly updated/checked, please ignore! 


## Main to-do
1. PSI4 templates can only set up SAPT/FSAPT/ISAPT input files. Geometry optimization templates need to make and tested
2. An approach that uses building schemes while including charged residues within a certain distance to seed
3. Interface with MBE codes
4. Interface with EnzyHTP
5. Formatting, plotting, and nomenclature standards

## problem cases where RINRUS may not behave correctly
1. Canonical residues in seed/ligand needs to be checked (C_alpha atoms should not be frozen, polypeptides and termini should be trimmed properly)
2. Salt bridges are not automatically added
3. Coding the automatic and correct treatment of cysteine S-S bridges is in progress
4. N or C-terminus residue could have a mis-named proton on carboxylic acid (MOE names is H which clashes with the N-H proton name)
5. RINRUS cannot automatically unfold symmetry of old multimeric PDB files - user may be on the hook for this anyway, since it's a preprocessing thing. TJ wrote a script to do this. I don't see it in the repo though
6. RINRUS may not correctly calculate seed charge if charged canonical amino acids are included in the seed
7. If # of waters (from MD simulation) exceeds 10,000, there can be issues with RINRUS. VMD will start printing the res# values in hexadecimal format, which will crash RINRUS. We have two scripts in /preprocessing to deal with this

## things in my list that Domi may already have fixed
1. Recognizing or ignoring duplicate atom types or res id #s in the pdb file
2. Canonical amino acids with invalid atom types
    * DAW: maybe we need rudimentary pdb check feature to read through initial file and check for duplicate atoms/res ids/non-standard atom types/etc. not actually fix them but flag them. also maybe flag non-canonical residues if user wants to add to res dictionary so they're split into MC and SC separately?
3. ~~Listing residues/fragments that aren’t in seed but must be included in all models (possible both manually and with the driver)~~
4. ~~Poorly justified element names truncating 2nd character of 2-character elements~~
5. ~~Improved documentation for frozen atom algorithm I wrote this a long time ago, but I don't remember the context: "frozen atom code has a slightly complicated interaction if MC or SC or both are in the model for carbon betas"~~ DAW fixed frozen atom stuff so this is prob fine
6. ~~Documentation for manually creating res_atoms.dat~~ covered in software paper

## documentation needed
* ~~how scripts work, inputs and outputs, etc~~
* ~~`res_atoms.dat` - how it is constructed and how it is used by the trimming script (already mentioned above)~~
* ~~detailed trimming rules incl. how frozen atoms selected etc.~~
* ~~why template pdb is useful~~
* ~~how current version differs from old versions - FG vs res partitioning and how that impacts models etc etc~~
* should we encourage users to add their non-standard residues to the rinrus libraries etc?
* brute force ways of making rinrus do stuff - splitting cofactor into separate residue ids etc

## examples
* ~~all examples need to be tested (and probably updated) to make sure they're consistent with the current version of rinrus/show off features in last few years~~
* set of prepped protein pdbs on github and range of driver inputs (maybe as code blocks in tutorial section of this wiki rather than lots of extra files in repo) so users can run and see usage/output easily rather than just looking at existing outputs?
* things to demonstrate:
    * ~~FG-sorted probe model building~~
    * ~~FG-sorted arpeggio model building~~
    * ~~FG-sorted dist model building~~
    * psi4 F-SAPT input prep and then output analysis
    * orca input and xyz>pdb conversion
    * must-add feature
