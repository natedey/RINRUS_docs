---
title: pdb_clean
layout: default
parent: "Individual functions"
---
# pdb_clean.py

PDB cleaning script from Arpeggio, see their documentation for more details

## Help message

```
usage: pdb_clean.py [-h] [-rmw] [-kh] [-if] pdb

#############
# CLEAN PDB #
#############

A program for cleaning PDB files.

Dependencies:
- Python (v2.7)
- BioPython (>= v1.60)

positional arguments:
  pdb                   Path to the PDB file to be cleaned.

options:
  -h, --help            show this help message and exit
  -rmw, --remove-waters
                        Remove waters.
  -kh, --keep-hydrogens
                        Keep hydrogens.
  -if, --informative_filenames
                        Keep a record of the flags used for cleaning the output filename.
```