---
title: MD_batch_rin
layout: default
parent: "Individual functions"
---

# MD_batch_rin.py

This function analyses the RINs of a set of MD frames and collects the data. It calculates summary statistics of the contact counts and prepares a "batch res atoms" file that selects all groups with contacts in any of the frames for making models with the same composition.

## Help message
```
usage: MD_batch_rin.py [-h] [-s SEED] [-dir DIR] [-type RINTYPE] [-addwat]

batch contact analysis of MD frames

options:
  -h, --help           show this help message and exit
  -s SEED, -seed SEED  seed ch:ID[,ch:ID,ch:ID]
  -dir DIR             directory to process
  -type RINTYPE        type of processing: probe/arpeggio
  -addwat              add waters to batch rin for each frame
```

## Inputs

- `-s`: seed, specified and used exactly the same way as in individual processing
- `-dir`: directory of PDB files to process, default is current directory
- `-type`: which of probe and arpeggio to use to generate the RINs
- `-addwat`: include the median number of waters in the batch_res_atoms files
    - off by default for now because distance analysis can be slow and so far we've mainly used this function for looking at the summary statistics

## Outputs

- `batch_res_atoms.dat`: res atoms file containing all non-water groups that have contacts in any of the given frames.
    - if `-addwat` used, a `batch_res_atoms.[frame].dat` file will be created for each frame with the median number of waters added as well
- `batch_stats.dat`: summary statistics table of contact counts for each identified functional group
- `batch_rawdf.pkl`: python pickle file containing pandas dataframe with all contact count info for every functional group in every frame. can be used for data analysis/plotting/etc
- `batch_atominfo.pkl`: lists of selected atoms for each functional group used to create batch_res_atoms.dat
- `waters_per_frame.dat`: list of how many water molecules have contacts in each of the given frames
