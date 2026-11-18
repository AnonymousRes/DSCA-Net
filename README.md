# DSCA-Net: Predicting Prescriptions via Dual Sequences with Cross Attention Network

<p align="center">
  <img src="DSCA-Net.png" alt="DSCA-Net Architecture" width="888">
</p>

<p align="center">
  <a href="https://doi.org/10.1109/BIBM49941.2020.9313492">
    <img src="https://img.shields.io/badge/IEEE_BIBM-2020-blue" alt="IEEE BIBM 2020">
  </a>
  <img src="https://img.shields.io/badge/TensorFlow-2.2.0-orange" alt="TensorFlow">
  <img src="https://img.shields.io/badge/Keras-2.3.1-red" alt="Keras">
  <img src="https://img.shields.io/badge/License-See_Repository-lightgrey" alt="License">
</p>

> **[Predicting Prescriptions via DSCA-Dual Sequences with Cross Attention Network](https://doi.org/10.1109/BIBM49941.2020.9313492)**  
> Wu Lee, Yuliang Shi, Lin Cheng, Yongqing Zheng, Zhongmin Yan  
> *2020 IEEE International Conference on Bioinformatics and Biomedicine (BIBM), pp. 615–622, 2020.*

## Overview

This repository provides the official implementation of **DSCA-Net (Dual Sequences with Cross Attention Network)**, proposed in our IEEE BIBM 2020 paper.

DSCA-Net is designed for prescription prediction using longitudinal electronic health records (EHRs). The model leverages dual sequences and cross-attention mechanisms to capture temporal dependencies and interactions among clinical information, supporting medication prediction from historical patient records.

The experiments are conducted using the **MIMIC-III** critical care database.

## Requirements

The original implementation was developed using the following environment:

| Package | Version |
|---|---|
| Python | Compatible with the dependencies below |
| TensorFlow | 2.2.0 |
| Keras | 2.3.1 |
| NumPy | 1.17.4 |
| Scikit-learn | 0.22 |

Additional dependencies include `pandas` and `fancyimpute`.

We recommend using an isolated environment with compatible package versions to reproduce the original experiments.

## Dataset

### MIMIC-III Database

DSCA-Net uses the publicly available **MIMIC-III** database, a large-scale critical care dataset containing de-identified clinical records.

The original MIMIC-III database can be obtained from:

**[MIMIC-III — PhysioNet](https://physionet.org/content/mimiciii/1.4/)**

Access to the original database is subject to PhysioNet credentialing and data-use requirements.

### Processed Data

For convenience and reproducibility, the project-related data files are available through Google Drive:

**[DSCA-Net Data — Google Drive](https://drive.google.com/drive/folders/1w89CddvA8t8KwPOPi-KcgIZivNx61OmL)**

The preprocessing pipeline uses the following files:

| File | Description |
|---|---|
| `lw_dataset_100.csv` | Clinical records and prescription-related features extracted from MIMIC-III |
| `hadm_days_100.csv` | Hospital admission identifiers used to organize longitudinal sequences |

These files serve as inputs to the Python preprocessing pipeline.

## Data Preprocessing

The preprocessing procedure consists of two stages: extracting clinical information from MIMIC-III and constructing the model-ready sequence data.

### Step 1. Build the MIMIC-III Database

First, download MIMIC-III and construct the database following the official MIMIC project instructions.

The database construction documentation is available from PhysioNet.

### Step 2. Extract Clinical Features

Use the provided SQL script:

`MIMIC-III_Data_Preprocess.sql`

This script documents the extraction and organization of clinical features from the MIMIC-III database.

The primary extracted dataset is:

`lw_dataset_100.csv`

The subsequent preprocessing also requires the corresponding admission identifier file:

`hadm_days_100.csv`

Alternatively, obtain the project-related data files from the Google Drive link above, subject to applicable data-use requirements.

### Step 3. Generate Model-Ready Sequences

Run the Python preprocessing script:

```bash
python data_process_mimic.py
```

The script performs the following operations:

1. Removes the `startdate` column and applies one-hot encoding to categorical demographic features.
2. Performs missing-value imputation using KNN with `k=10`.
3. Groups clinical records by hospital admission identifier (`hadm_id`).
4. Constructs longitudinal input sequences and next-step prescription prediction targets.
5. Saves the processed sequences as NumPy arrays.

The main generated files are:

| File | Description |
|---|---|
| `lw_dataset_onehot_100.csv` | One-hot encoded clinical data |
| `lw_dataset_onehot_knnimpute_100.csv` | Clinical data after KNN imputation |
| `knnimpute_dfdfinal_100.npy` | Imputed numerical feature matrix |
| `dualseqs_down_100.npy` | Input sequences for model training |
| `seqy_down_100.npy` | Corresponding prescription prediction targets |

The model-ready input and target files are:

```text
dualseqs_down_100.npy
seqy_down_100.npy
```

**Note:** Update the dataset paths in `data_process_mimic.py` according to your local environment before running the script.

## DSCA-Net Training

After preprocessing the dataset, run the main training program:

```bash
python main.py
```

The training program uses the processed longitudinal sequences to train and evaluate DSCA-Net for prescription prediction.

Please refer to the source code for the original model configuration, training settings, and evaluation procedures.

## Reproduction Guidelines

To reproduce the experiments reported in the paper, follow the workflow below:

```text
MIMIC-III Database
        |
        v
MIMIC-III_Data_Preprocess.sql
        |
        v
lw_dataset_100.csv
        |
        +---- hadm_days_100.csv
        |
        v
data_process_mimic.py
        |
        v
One-Hot Encoding
        |
        v
KNN Missing-Value Imputation
        |
        v
Longitudinal Sequence Construction
        |
        v
dualseqs_down_100.npy
seqy_down_100.npy
        |
        v
main.py
        |
        v
DSCA-Net Training and Evaluation
```

For reproducibility, we recommend retaining the original preprocessing procedure and experimental configuration when comparing against the published results.

## Notes

- The original implementation uses TensorFlow 2.2.0 and Keras 2.3.1. Newer versions may require compatibility adjustments.
- The preprocessing scripts contain local file paths that should be updated before execution.
- The preprocessing pipeline uses hospital admission identifiers to construct patient-level temporal sequences.
- Researchers using MIMIC-III must comply with the applicable PhysioNet data-use agreement.

## Citation

If you find this repository or our work useful in your research, please cite:

```bibtex
@INPROCEEDINGS{9313492,
  author={Lee, Wu and Shi, Yuliang and Cheng, Lin and Zheng, Yongqing and Yan, Zhongmin},
  booktitle={2020 IEEE International Conference on Bioinformatics and Biomedicine (BIBM)},
  title={Predicting Prescriptions via DSCA-Dual Sequences with Cross Attention Network},
  year={2020},
  pages={615-622},
  doi={10.1109/BIBM49941.2020.9313492}
}
```

## Contact

If you have any questions about the implementation or experiments, please feel free to open a GitHub issue or contact the authors through the repository.

