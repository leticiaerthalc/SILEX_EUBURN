# SILEX Atmospheric Layer Classification

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24529089.svg)](https://doi.org/10.5281/zenodo.24529089)

Flight-by-flight classification of the atmospheric layers sampled during all
flights of the SILEX aircraft campaign ([dates, location, aircraft]), produced
within the [EUBurn](https://euburn.aeris-data.fr/) project.

## Files

| File | Method | Description |
|------|--------|-------------|
| `SILEX_classification_v1.ods` | Manual | Version 1: all layers, expert-based |
| `SILEX_classification_v2.ods` | Manual + K-Means | Version 2: the manual classification (unchanged from v1) plus the K-Means classification, which validates it and describes the in-plume moments more specifically |

Each file contains a "Flight details" page describing what was measured in the
atmospheric layers of each flight.

## Methodology

### Version 1: manual identification

1. **Logbook:** notes from the SILEX logbook are compiled to locate the
   periods of interest.
2. **Detection of potential plumes:** peaks in black carbon (BC) mass
   concentration are identified in the time series.
3. **Plume classification:** plumes are classified from their particle size
   distribution:

   | Modal diameter | Plume type |
   |----------------|------------|
   | 40–150 nm | Fossil fuel (FF) |
   | 100–200 nm | Biomass burning (BB), French sources |
   | > 200 nm (far from local sources) | Long-range transport (LRT) |

4. **Source and age:** HYSPLIT back-trajectories, computed from the aircraft
   coordinates, are used to trace the origin of LRT plumes.

**v1 categories**

| Category | Description |
|----------|-------------|
| Airport | Primary emissions captured during landing and take-off (LTO) cycles |
| Background | Ambient baseline established during the initial cruise phase |
| Local Plume | BC spikes with smaller aerosol diameters |
| LRT Plume | BC spikes with larger aerosol diameters |
| Diluted | Interfacial mixing zones at the plume–background boundary |
| Mix | Mixtures of plane emissions with the background or pre-existing plumes |

### Version 2: manual classification + K-Means clustering

Version 2 keeps the manual classification of version 1 unchanged and adds an
unsupervised K-Means classification. K-Means is used to validate the manual
labels and to describe the in-plume moments more specifically (diluted
moments of the plume are separated from the core of the plume).

1. **Input** (airport data excluded):
   - UHSAS: size distribution (dV/dlogDp, volume fraction), N_total, V_total
   - SP2: BC mass
   - Ratios: CO/CO₂, CH₄/CO, O₃/CO
2. **Pre-processing:** log transform to compress extreme values,
   standardisation of all variables to the same scale, and BC weighted ×3 so
   that black carbon is the main driver of the clusters.
3. **K-Means:** one model per domain: local air (K = 2) and other air (K = 4).
4. **Clusters:** 6 clusters, interpreted from their particle properties and
   chemical ratios.

**Cross-validation:** cluster labels are overlaid on the manually flagged
plumes along each flight (BC and altitude time series). Agreement is
quantified with the adjusted Rand index (ARI) and a confusion matrix.

**v2 clusters** ([mean / median] values per cluster)

| Cluster | Description | 
|---------|-------------|
| C0 | Clean air / background | 
| C1 | Polluted background |
| C2 | Mixed air |
| C3 | Diluted local plume | 
| C4 | Aged smoke (LRT) | 
| C5 | Intense local plume |

> **Not included:** the K-Means background clusters (C0, C1, C2) are not
> distributed here; please contact the author to request them. Only the
> in-plume clusters (**C3, C4 and C5**) are provided.
>
> The manual classification is identical in v1 and v2. Flights 23, 28, 30 and
> 31 have no K-Means result, because the method did not flag any plumes in
> them; for these flights only the manual classification is available.

**Abbreviations:** BC: black carbon · FF: fossil fuel · BB: biomass burning ·
LRT: long-range transport · ARI: adjusted Rand index · LTO: landing and take-off

## Usage

```python
import pandas as pd  # requires: pip install odfpy

path = "SILEX_classification_v2.ods"
print(pd.ExcelFile(path, engine="odf").sheet_names)  # list the sheets

df = pd.read_excel(path, sheet_name="[sheet name]", engine="odf")
```

## Citation

If you use this dataset, please cite:

> Erthal Cordeiro, L., Velazquez Garcia, A., & Denjean, C. (2026). SILEX
> atmospheric layer classification: flight-by-flight manual and K-Means (v2)
> (Version 2) [Dataset]. Zenodo. https://doi.org/10.5281/zenodo.24529089

The associated PhD thesis is in preparation; this section will be updated with
its reference once it is published.

## License

Released under [CC BY 4.0 / exact variant]. See the repository
[`LICENSE`](../LICENSE).

## Acknowledgements

This work is part of the EUBURN (the southern EUrope Biomass BURNing) research
programme led by Météo-France, supported by the French National Research Agency
(ANR, grant no. ANR-24-CE01-3132), CNRS (LEFE-INSU programme), CNES, and
France 2030 (Clermont Auvergne CIR4 project). Thanks to Pierre Nabat and
Olivier Clary.

## Contact

Leticia Erthal Cordeiro – leticia.erthalcordeiro@meteo.fr – CNRM

## Changelog

- **v2** (September 2026): added the K-Means classification (in-plume clusters C3–C5) and its cross-validation against the manual classification; the manual classification is unchanged
- **v1** (April 2026): initial release, manual classification
