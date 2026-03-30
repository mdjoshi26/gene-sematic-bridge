# gene-sematic-bridge
# Gene-Semantic Bridge
### Enhancing Single-Cell Perturbation Prediction via LLM-Knowledge Embeddings

> **Status: Active Research** — Currently training and evaluating on Norman et al. (2019). Replogle et al. (2022) benchmarking in progress.

---

## What is this?

Standard perturbation models treat genes as arbitrary IDs — "Gene_001" — ignoring decades of biological literature. **Gene-Semantic Bridge** closes that gap by encoding what each gene *functionally does* using BioBERT embeddings, then fusing that semantic knowledge with a Conditional VAE to predict post-perturbation gene expression — including for genes never seen during training.

---

## How it works

1. **LLM Embeddings** — Functional descriptions fetched from NCBI Gene / MyGene.info, encoded into 768-dim vectors via BioBERT
2. **Perturbation Encoder** — Fuses BioBERT vectors with learnable gene-identity embeddings → 128-dim condition vector
3. **KA-CVAE** — Conditional VAE that takes the condition vector + cell expression → reconstructs post-perturbation expression profile

```
Gene name → BioBERT → 768-dim semantic vector ─┐
                                                 ├→ PerturbationEncoder → 128-dim condition
Condition ID → learnable 64-dim embedding ──────┘
                                                        ↓
Cell expression → ExpressionEncoder → z (128-dim latent)
                                                        ↓
                              ExpressionDecoder → predicted expression (5045 genes)
```

---

## Dataset

**Norman et al. (2019)** — K562 cell line
- 91,205 single cells × 5,045 genes
- 152 single-gene + 131 dual-gene perturbations
- 4 benchmark splits: `simulation`, `combo_seen0`, `combo_seen1`, `combo_seen2`

---

## Current Results

| Split | Pearson r (top-20 DEGs) | R² | MSE |
|---|---|---|---|
| combo_seen2 | 0.2035 | >0.996 | ~0.044 |
| simulation | 0.1087 | >0.996 | ~0.043 |
| combo_seen1 | 0.0522 | >0.996 | ~0.046 |
| combo_seen0 | 0.0114 | >0.996 | ~0.047 |

Performance degrades as gene observability decreases — expected, and the core challenge this work addresses.

---

## What's Next

- [ ] **Replogle et al. (2022) benchmarking** — 162k cells, 1092 perturbations, different cell line. Key test of generalizability
- [ ] **Improve OOD performance** — combo_seen0 (neither gene seen during training) is the hard frontier
- [ ] **Better embeddings for missing genes** — 6 genes had no database entries and fell back to generic text; exploring alternative sources
- [ ] **Scaling experiments** — larger latent dim, deeper decoder, attention-based fusion
- [ ] **Cross-dataset evaluation** — validate that the approach transfers across cell lines without dataset-specific tuning

---

## Stack

`Python` · `PyTorch` · `BioBERT` · `scRNA-seq` · `VAE` · `MyGene.info` · `NCBI Gene`

---
