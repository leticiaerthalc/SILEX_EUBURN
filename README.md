# PhD Thesis Data Products

Data products produced during my PhD thesis within the
[EUBurn](https://euburn.aeris-data.fr/) project.

## Context

- **Project:** EUBurn – [The southern Europe biomass burning project]
- **Thesis:** [Characterization of aerosols released by  wildfires in Europe and their regional climate impact ], [Université de Toulouse/MétéoFrance - CNRM], [2028]
- **Author:** [Leticia ERTHAL CORDEIRO] ([https://orcid.org/0000-0001-8559-7851])

## Citation

If you use this dataset, please cite:

> Erthal Cordeiro, L., Velazquez Garcia, A., & Denjean, C. (2026). SILEX atmospheric layer classification: flight-by-flight manual and K-Means (v2) (Version 2) [Dataset]. Zenodo. https://doi.org/10.5281/zenodo.24529089

## License

This work is licensed under [CC BY 4.0 / the variant in your LICENSE file].
See [`LICENSE`](https://github.com/leticiaerthalc/SILEX_EUBURN/blob/main/LICENSE).

## Repository structure

```
.
├── Atmospheric_Layer_Classification/
│   ├── SILEX_classification_v1.ods
│   └── SILEX_classification_v2.ods
├── LICENSE
└── README.md
```

| Folder / File | Description |
|---------------|-------------|
| `Atmospheric_Layer_Classification/` | Flight-by-flight classification of the atmospheric layers sampled during SILEX (v1 and v2) |
| `LICENSE` | Creative Commons license terms |

More products will be added as separate folders.

---

## Product 1: Atmospheric Layer Classification

Flight-by-flight classification of the atmospheric layers sampled during all
flights of the SILEX campaign ([dates, location, aircraft, instruments]).

| File | Method | Description |
|------|--------|-------------|
| `SILEX_classification.ods` | Manual | Version 1: all layers, expert-based |
| `SILEX_classification_v2.ods` | K-Means (unsupervised) | Version 2: validated and more specific; **in-plume moments only** |

Each file contains a "Flight details" page describing what was measured in
the atmospheric layers of each flight.

### Methodology

#### Version 1: manual identification

1. **Logbook:** notes from the SILEX logbook are compiled to locate
   the periods of interest.
2. **Detection of potential plumes:** peaks in black carbon (BC) mass
   concentration are identified in the time series.
3. **Plume classification:** plumes are classified from their particle size
   distribution:

   | Modal diameter | Plume type |
   |----------------|------------|
   | 40–150 nm | Fossil fuel (FF) |
   | 100–200 nm | Biomass burning (BB), French sources |
   | > 200 nm (and far from local sources) | Long-range transport (LRT) |

4. **Source and age:** HYSPLIT back-trajectories, computed from the
   aircraft coordinates, are used to trace the origin of LRT plumes.

**v1 categories**

| Category | Description |
|----------|-------------|
| Airport | Primary emissions captured during landing and take-off (LTO) cycles |
| Background | Ambient baseline established during the initial cruise phase |
| Local Plume | BC spikes with smaller aerosol diameters |
| LRT Plume | BC spikes with larger aerosol diameters |
| Diluted | Interfacial mixing zones at the plume–background boundary |
| Mix | Mixtures of plane emissions with the background or pre-existing plumes |

#### Version 2: K-Means clustering (unsupervised)

1. **Input** (airport data excluded):
   - UHSAS: size distribution (dV/dlogDp, volume fraction), N_total, V_total
   - SP2: BC mass
   - Ratios: CO/CO₂, CH₄/CO, O₃/CO
2. **Pre-processing** (common feature space): log transform to compress
   extreme values, standardisation of all variables to the same scale, and
   BC weighted ×3 so that black carbon is the main driver of the clusters.
3. **K-Means:** one model per domain: local air (K = 2) and other air (K = 4).
4. **Clusters:** 6 clusters, interpreted from their particle properties and
   chemical ratios.

**Cross-validation:** cluster labels are overlaid on the manually flagged
plumes along each flight (BC and altitude time series). Agreement is
quantified with the adjusted Rand index (ARI) and a confusion matrix.

**v2 clusters**

| Cluster | Description | Dp vol [nm] | N_total [cm⁻³] | CO/CO₂ | CH₄/CO | NOx/CO | NOx_100 | BC mass |
|---------|-------------|-------------|----------------|--------|--------|--------|---------|---------|
| C0 | Clean air / background | 98.18 | 39.9 | 0.177 | 26.86 | 0.0122 | 0.92 | 0.00 |
| C1 | Polluted background | 263.03 | 347.3 | 0.223 | 22.07 | 0.0122 | 1.10 | 0.02 |
| C2 | Mixed air | 263.03 | 493.1 | 0.279 | 17.95 | 0.0103 | 1.16 | 0.04 |
| C3 | Diluted local plume | 263.03 | 1152.3 | 0.271 | 18.43 | 0.0147 | 1.59 | 0.09 |
| C4 | Aged smoke (LRT) | 398.79 | 674.6 | 0.542 | 9.49 | 0.0062 | 1.30 | 0.26 |
| C5 | Intense local plume | 263.03 | 5244.7 | 0.779 | 7.78 | 0.0095 | 2.54 | 1.17 |

> **Note:** `SILEX_classification_v2.ods` only includes the in-plume moments
> (**C3, C4 and C5**). The background clusters (C0, C1, C2) are not
> distributed here; please contact me to request them.
>
> Flights 23, 28, 30 and 31 keep the manual classification, because the
> K-Means method did not flag any plumes.

**Abbreviations:** BC: black carbon · FF: fossil fuel · BB: biomass burning ·
LRT: long-range transport · ARI: adjusted Rand index · Dm: modal diameter


### Usage

```python
import pandas as pd  # requires: pip install odfpy

df = pd.read_excel(
    "Atmospheric_Layer_Classification/SILEX_classification_v2.ods",
    engine="odf",
)
```

---

## Citation

If you use this dataset, please cite:

> Erthal Cordeiro, L., Velazquez Garcia, A., & Denjean, C. (2026). SILEX atmospheric layer classification: flight-by-flight manual and K-Means (v2) (Version 2) [Dataset]. Zenodo. https://doi.org/10.5281/zenodo.24529089

## License

This work is licensed under [CC BY 4.0 / the variant in your LICENSE file].
See [`LICENSE`](https://github.com/leticiaerthalc/SILEX_EUBURN/blob/main/LICENSE).

## Acknowledgements

This work is part of the EUBURN (the southern EUrope Biomass BURNing) research programme led by Météo-France, supported by the French National Research Agency (ANR, grant no. ANR-24-CE01-3132), CNRS (LEFE-INSU programme), CNES, and France 2030 (Clermont Auvergne CIR4 project).
Thanks to Cyrielle Deanjean, Pierre Nabat, Alejandra Velazquez Garcia and Olivier Clary.

## Contact

Leticia Erthal Cordeiro – leticia.erthalcordeiro@meteo.fr – CNRM

## Changelog

- **v2** (September 2026): Inclusion of K-Means evaluation
- **v1** (April 2026): initial release
