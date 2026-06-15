---
title: probe2rins
layout: default
parent: "Individual functions"
---

# probe2rins.py

`probe2rins.py` analyses a Probe output file, extracts all contacts involving any seed atoms, and prepares `res_atoms.dat` files for the model trimming.

## Help message
```
usage: probe2rins.py [-h] [-f PROBEFILE] [-s SEED]

Generate interaction information from probe file. Usage: probe2rins.py -f probe_file -s seed

options:
  -h, --help           show this help message and exit
  -f PROBEFILE         probe_file
  -s SEED, -seed SEED  seed for select RIN, in the format of "A:300,A:301,A:302"
```

## Inputs
- `-f`: Probe contacts file
  - Given as filename
  - Example: `-f 2cht_h.probe`
- `-s`: Seed
  - Given as ch:ID pairs
  - Examples: `-s A:203` or `-seed A:301,A:302`

## Outputs
- `FG_probe_counts.dat`: table listing probe contact counts with seed for each FG
- `res_atoms.dat`: res\_atoms file ranked by total contact count