---
title: Structure prep
layout: default
parent: "How to use RINRUS"
nav_order: 1
---

# Preparing your system for RINRUS

## Preparing structure files
If your structure is not appropriately pre-processed then go through the following steps to protonate and clean up substrate molecules. This is the user's responsibility to verify, since RINRUS will generate models from provided structure and cannot yet run sanity checks.

1. After getting a raw PDB file (`3bwm.pdb`), check for nonstandard fragments and clean up atoms or residues with alternate positions/conformations. Listed below are some possible tasks that need to be done before starting the workflow:

    a) If there are multiple conformations for a residue then keep only one conformation.

    b) If starting from an MD simulation, check that metal coordination is correct. You may also want to delete neutralizing counterions if they have migrated into the active site.

    c) If crystallographic symmetry is embedded into the pdb, one needs to be very careful and manually "unfold" the multimers from the PDB website. This might be more common with older X-ray crystal structures. RINRUS will eventually be improved to recognize this and automate symmetry unpacking. 

    d) We have seen situations where AMBER truncates atom names of two-letter elements when writing to PDB format (turning FE into F for example). If the PDB file comes from a source other than PDB repository or similar database, check all atom names, residue names, and residue IDs.

2. If the protein is not yet protonated, run `reduce` to generate a new protonated PDB file  (`3bwm_h.pdb`) or protonate with other program of your choice. Skip this step if the pdb file is from an MD simulation since it has been protonated already.
    ```bash
    # Example usage of reduce
    $HOME/git/RINRUS/bin/reduce -NOFLIP 3bwm.pdb > 3bwm_h.pdb 
    ``` 

3. Check the new PDB file. Probe doesn't recognize covalent bonds between fragments, which becomes a problem with metalloenzymes and metal-ligand coordination. Our workaround is to replace a metal coordination center with an atom that probe recognizes as a H-bond donor or acceptor to capture interactions with the coordinating ligand atoms. For example in COMT, we replace Mg with O, and save as a new PDB file (`3bwm_h_modify.pdb`).

4. Check all ligands, make sure H atoms were added correctly (may need to delete or add more H based on certain conditions). The user needs to protonate ligand and substrate properly since reduce does not recognize most substrates and may add H improperly. Check the ligand 2D drawing for the PDB entry on the RCSB website for some guidance on substrate protonation if starting from scratch. Performing this step correctly is the user's responsibility! RINRUS will generate models from provided structure and does not yet have comprehensive sanity checks.

## Defining the seed

***Defining the "seed" is one of the most important steps of the QM-cluster model building process.*** What should you select as the seed? Typically, the seed will be the substrate(s) (or ligand in biochemical terms) participating in the chemical reaction. Any amino acid residues, co-factors, or fragments which participate in the active site catalytic breaking and forming of chemical bonds may also need to be included as part of the seed, but this will generate much larger models compare to only using the substrate.

The seed is specified as a comma-separated list of colon-separated Chain:ResidueID pairs. In the example of 3BWM, we select the seed as A:300 (Mg2+), A:301 (SAM) and A:302 (catechol). 

If the PDB you're using does not have chain identifiers, you will need to specify ":XXX" where XXX is the residue ID number in this step and beyond. Our current defaults are wonky in these cases and need to be improved. If the protein is multimeric, use the chain of your choice for seed fragments. Note that some multimeric x-ray crystal structures may not necessarily have equivalent active sites!
