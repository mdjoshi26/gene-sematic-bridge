**Gene-Semantic Bridge: Enhancing Cell Perturbation Prediction via LLM-Knowledge Embeddings**

KA-CVAE is a knowledge-augmented conditional variational autoencoder designed to predict single-cell transcriptomic responses to single and combinatorial CRISPR perturbations. The project bridges the gap between biological knowledge and perturbation modeling by integrating **BioBERT-derived semantic gene embeddings** with learnable gene identity representations.

Unlike traditional approaches such as CPA and GEARS that treat genes as categorical IDs, KA-CVAE injects functional information from biomedical literature into the perturbation representation space, enabling improved generalization to unseen perturbation conditions.

## Features
- BioBERT-based semantic gene embedding pipeline
- Knowledge-augmented CVAE architecture
- Support for single and dual-gene perturbation prediction
- Out-of-distribution evaluation using GEARS benchmark splits
- Cross-dataset evaluation on Replogle et al. (2022)
- KL warmup to prevent posterior collapse
- PCA-based analysis of semantic gene embedding space

## Architecture
KA-CVAE consists of:
- **PerturbationEncoder** – combines semantic BioBERT embeddings with learnable gene identity embeddings
- **ExpressionEncoder** – maps perturbed expression profiles into a latent space
- **ExpressionDecoder** – reconstructs post-perturbation transcriptomic profiles

## Datasets
- **Norman et al. (2019)** CRISPRa dataset  
  - ~91k cells  
  - 284 perturbation conditions  
  - 5,045 genes  

- **Replogle et al. (2022)** dataset for OOD evaluation  
  - ~162k cells  
  - 1,093 perturbation conditions  

## Results
KA-CVAE demonstrates improved out-of-distribution generalization compared to baseline approaches, including non-zero performance in the hardest unseen combinatorial perturbation setting (`combo seen0`). The model also achieves strong cross-dataset transfer performance on Replogle without retraining.

## Tech Stack
Python, PyTorch, BioBERT, Transformers, Pandas, NumPy, scRNA-seq processing pipelines

## References
- CPA (Lotfollahi et al., 2021)
- GEARS (Roohani et al., 2023)
- BioBERT (Lee et al., 2020)
- scVI (Lopez et al., 2018)
