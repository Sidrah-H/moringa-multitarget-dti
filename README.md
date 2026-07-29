# moringa-multitarget-dti
Multi-target ML-based drug–target interaction prediction of Moringa oleifera phytochemicals against MAO-B, AChE, and BACE1 using DeepPurpose
**Multi-Target DTI Prediction of Moringa oleifera Phytochemicals**
Machine learning-based drug–target interaction (DTI) screening of 12 Moringa oleifera-derived phytochemicals (benchmarked against selegiline) across three neurodegeneration-relevant protein targets: MAO-B, acetylcholinesterase (AChE), and BACE1.
This is a follow-up side project to my single-target molecular docking + ADMET study on the same compound library against MAO-B (docking/ADMET manuscript submitted to IJPSN — citation added here once published). Where that study used physics-based docking against one target, this project extends the screening to a multi-target, ML-based approach using DeepPurpose, and compares the two methods' outputs.
**Motivation**
Neurodegenerative disease drug discovery increasingly favors multi-target-directed ligands (MTDLs) over single-target inhibitors, since diseases like Alzheimer's and Parkinson's involve multiple interacting pathological pathways. This project asks: do phytochemicals already showing promise against MAO-B also show predicted activity against AChE and BACE1 — two other validated neurodegeneration targets?
**Methods**
Targets: MAO-B (UniProt P27338), AChE (UniProt P22303), BACE1 (UniProt P56817) — canonical sequences retrieved via the UniProt REST API.
Compounds: 12 Moringa-derived phytochemicals + selegiline (reference MAO-B inhibitor), SMILES retrieved from PubChem, cross-checked against the structures used in the prior docking/ADMET study.
Model: DeepPurpose pretrained `Morgan_CNN_BindingDB` model — Morgan (ECFP) fingerprint encoding for drugs, CNN encoding for target sequences.
Output: predicted binding affinity score (pKd-like scale; higher = stronger predicted interaction) for all 39 compound × target pairs.
Comparison: predicted DTI scores ranked against prior docking binding energies (ΔG, kcal/mol) for the same compounds using Spearman rank correlation, to assess agreement between structure-based and ML-based methods.
**Key results**
Isolariciresinol was the top-ranked hit in both methods (docking rank 2/13, DTI rank 1/13) — the strongest convergent finding across both independent approaches.
Selegiline (reference compound) ranked highly in the DTI model (rank 2/13) but comparatively lower in docking (rank 11/13), highlighting a notable divergence between methods for this compound.
3-p-Coumaroylquinic acid and 4-Caffeoylquinic acid were top docking hits but among the weakest DTI-predicted binders — likely reflecting the ML model's reliance on 2D fingerprint/sequence patterns learned from BindingDB, versus docking's 3D structural pocket-fit calculation.
Overall rank agreement between docking ΔG and DTI score was weak and not statistically significant (Spearman ρ = 0.379, p = 0.201, n = 13) — consistent with docking and ligand/sequence-based ML models capturing complementary but non-identical aspects of drug–target interaction, rather than being interchangeable predictors.
Full results: `results/DTI_predictions_by_target.csv`, `results/docking_vs_dti_comparison.csv`
**Repository structure**
```
├── notebook/
│   └── Moringa_MultiTarget_DTI_DeepPurpose.ipynb   # full, runnable Colab pipeline
├── data/
│   ├── compounds_smiles.csv        # compound names, PubChem CIDs, SMILES
│   ├── target_sequences.fasta      # MAO-B, AChE, BACE1 sequences
│   └── docking_dG_reference.csv    # binding energies from prior docking study
├── results/
│   ├── DTI_predictions_by_target.csv
│   └── docking_vs_dti_comparison.csv
└── figures/
```
