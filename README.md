# football-pace

Repo for two studies of **Pace of Play (PoP)** in elite international football. Both use StatsBomb
open event data. Paper [2] uses events and 360 freeze frames from six tournaments: UEFA Euro 2020
and 2024, FIFA World Cup 2022, UEFA Women's Euro 2022 and 2025, and FIFA Women's World Cup 2023.
Paper [1] uses the four women's tournaments from 2019 to 2025: FIFA Women's World Cup 2019 and 2023,
and UEFA Women's Euro 2022 and 2025.

## Notebooks

Run notebooks 1-4 in order, from inside that folder.

| Notebook                                  | What it does                                                                                                                                                                                                                                        | Paper |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----- |
| `1-load-and-convert-statsbomb-data.ipynb` | Downloads the StatsBomb open data (events + 360 frames) with socceraction's `StatsBombLoader` and saves each match to HDF5. Then computes four established PoP metrics for the women's tournaments 2019–2025 and fits the mixed models of the conference paper (Table 1, Figure 1, trends by metric) | [1]   |
| `2-Pace-of-Play-index.ipynb`              | Computes the possession-level PoP variables and the four baseline metrics (Table 1), standardises them, runs the EFA item screening and builds the Horst-weighted Pace of Play Index (PPI). Outputs Figure 1, Fig. S1 and Tables S4–S6 | [2]   |
| `3-build-pace-model-analysis.ipynb`       | Tests how sensitive the PPI is to the weighting rule (Table S1), compares counter vs regular play against the baselines (Table S2), estimates sex differences with mixed models, splits PPI differences into item contributions, and draws Figure 2 | [2]   |
| `4-tactical-clustering.ipynb`             | Derives pace states: compares clustering methods (Table S3), fits K-means with K = 2, bootstraps the boundary (95% CI), and outputs Figure 3, the pace-state profile with Youden cut-points (Table 2) and the EURO 2024 final case example (Figure 4) | [2]   |

## Data and outputs

The data are not included in this repository. They are freely available from Hudl StatsBomb at
<https://github.com/hudl/open-data>.

- Notebook 1 writes the raw match data to `data/men-statsbomb.h5` and `data/women-statsbomb3.h5`
  (used by notebooks 2–4), and to `data/women-statsbomb4.h5` (the four women's tournaments of [1]).
  Files that already exist are skipped. Set `OVERWRITE = True` to download them again.
- Notebook 2 writes `data/ppi-features.h5`, which contains three tables:
  - `features`: every possession, with all variables
  - `ppi`: the 39,995 analysed possessions, with the nine PPI items and the PPI itself
  - `horst_weights`: the item weights
- Notebook 4 adds the table `pace_states` to the same file.
- Figures and tables go to `figures/` and are named after the manuscript, e.g. `Figure3_…` or
  `TableS2_…`. Two figures that are not in the manuscript have no number
  (`EFA_vs_Horst_by_tournament.png`, `boundary_bootstrap.png`). The outputs of notebook 1 start
  with `ECSS_`.

Downloading the data in notebook 1 takes about 15 minutes, and its analysis about 2 minutes.
Notebook 2 takes about 5–10 minutes to run. Notebooks 3 and 4 take a few minutes each.

## Requirements

Python 3.12 with pandas 2.3, numpy 1.26, scipy, scikit-learn 1.7, statsmodels 0.14,
factor_analyzer 0.5.1, matplotlib, seaborn, PyTables (`tables`), tqdm, socceraction 1.5.3 and
statsbombpy.

## Citation

If you use notebook 1, please cite:

[1] Yang, J., & Kempe, M. (2026, July 7–10). _No evidence of accelerating pace in elite women's
football: A multi-method analysis across international tournaments_ [Conference paper]. 31st Annual
Congress of the European College of Sport Science (ECSS), Lausanne, Switzerland.
<https://www.researchgate.net/publication/416421532>

If you use notebooks 2–4 (the PoP variables, the PPI or the pace states), please cite:

[2] Yang, J., Wu, J., Robertson, S., & Kempe, M. (2026). _Measuring Pace of Play in Elite
Football: A Multidimensional Possession-Level Index_ [Manuscript submitted for publication].

## Acknowledgements

This project builds on [socceraction](https://github.com/ML-KULeuven/socceraction). We use its
StatsBomb data loader, and our notebook layout follows its public notebooks. We thank the
socceraction authors for making their work openly available:

> Decroos, T., Bransen, L., Van Haaren, J., & Davis, J. (2019). Actions speak louder than goals:
> Valuing player actions in soccer. _Proceedings of the 25th ACM SIGKDD International Conference on
> Knowledge Discovery & Data Mining_, 1851–1861. <https://doi.org/10.1145/3292500.3330758>

We also thank Hudl StatsBomb for providing the open data free of charge.
