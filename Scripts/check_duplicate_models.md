---
title: check_duplicate_models
layout: default
parent: "Individual functions"
---

# check_duplicate_models.py

## Help message
```
Compare pdbs to each other
usage:  check_duplicate_models.py model1.pdb model2.pdb ...
```

## Inputs

- list of pdb files taken as positional argument
  - Given as list of file names (can use wildcards etc)
  - Can be used to compare models within a sequence or made with different ranking schemes etc
  - Example: `check_duplicate_models.py model_*_template.pdb` or `check_duplicate_models.py probe_models/model_*_template.pdb arpeggio_models/model_*_template.pdb`


## Outputs

- prints which models from the given list are the same to the command line