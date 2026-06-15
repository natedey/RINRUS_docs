---
title: Arpeggio
layout: default
parent: "Individual functions"
---

# Arpeggio

Arpeggio (DOI:10.1016/j.jmb.2016.12.004) is a program for interatomic contact analysis that comes packaged with RINRUS

## Usage
```
python3 ~/git/RINRUS/bin/arpeggio/arpeggio.py
```

## Input

- PDB file

For other keywords see Arpeggio instructions

## Output

- [PDB].atomtypes
- [PDB].bs_contacts
- [PDB].contacts
- [PDB].specific.sift
- [PDB].sift
- [PDB].specific.siftmatch
- [PDB].siftmatch
- [PDB].specific.polarmatch
- [PDB].polarmatch
- [PDB].ri
- [PDB].rings
- [PDB].ari
- [PDB].amri
- [PDB].amam
- [PDB].residue_sifts

Only the [PDB].contacts file is needed for RINRUS.