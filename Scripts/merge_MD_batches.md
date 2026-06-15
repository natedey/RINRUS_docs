---
title: merge_MD_batches
layout: default
parent: "Individual functions"
---

# merge_MD_batches.py

This function compiles the outputs of [MD_batch_rin.py](MD_batch_rin.html) across multiple directories, so that entire trajectories can be broken up into sections and analysed "in parallel" for speed. 

Note that the MD_batch_rin outputs in each directory must have the standard file names.

## Help message
```
usage: merge_MD_batches.py [-h] [-s SEED] [-dirs [DIRS ...]] [-addwat]

merge data from batch processed dirs

options:
  -h, --help           show this help message and exit
  -s SEED, -seed SEED  seed ch:ID[,ch:ID,ch:ID]
  -dirs [DIRS ...]     directories to merge data from
  -addwat              add waters to batch rin for each frame
```

## Inputs

- `-s`: seed, specified and used exactly the same way as in individual processing
- `-dirs`: list of directories to combine MD outputs from
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
