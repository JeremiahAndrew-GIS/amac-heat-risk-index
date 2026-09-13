[README (1).md](https://github.com/user-attachments/files/32174029/README.1.md)# AMAC Heat Risk Index

Which of AMAC's 12 wards face the highest heat risk, and can that answer keep itself current without manual GIS work every time someone asks?

Built over twelve months with GeoDev Lab Africa, Cohort 2.

## The question

This project scores all 12 wards in Abuja Municipal Area Council (AMAC) City Center 1, Garki 1, Gui, Gwagwa, Gwarinpa, Jiwa, Kabusa, Karshi 1, Karu, Nyanya 1, Orozo, and Wuse  on a composite heat risk index, combining:

- **Exposure**  how hot a ward actually gets (LST, NDBI, NDVI)
- **Sensitivity**  who and what is most affected by that heat (population, population 65+ and under 5, household size, building/settlement density)
- **Adaptive capacity**  how well-equipped a ward is to cope (health facility access, road density, green space)

Full reasoning and datasets are in [`data-notes.md`](./data-notes.md).

## Why it matters

I answered a version of this question once before, by hand, in my undergraduate thesis on urban heat island dynamics in Abuja. That answer lived in a folder and needed redoing manually every time. This time the goal is a system: same rigor, but it re-runs on its own.

## Datasets used

| Dataset | Source | Role |
|---|---|---|
| Operational Wards v3.0 | [GRID3](https://data.grid3.org) | Study area  AMAC's 12 wards |
| AMAC/state boundaries | [GADM v4.1](https://gadm.org) | Boundary cross-check |
| Health Facilities v3.0 | [GRID3](https://data.grid3.org) | Adaptive capacity |
| Settlement Extents v4.1 | [GRID3](https://data.grid3.org) | Sensitivity (density, building metrics) |
| Roads v1.0 (primary) | [GRID3](https://data.grid3.org) | Adaptive capacity (road density) |
| OSM roads | Overpass API, via OSM Extractor | QA cross-check only  not fed into the index |
| OSM buildings | Overpass API, via OSM Extractor | Sensitivity |

Full source links, feature counts, columns, and data quality notes for every layer are in [`data-notes.md`](./data-notes.md).

## What I'm building toward

A Python pipeline (`ee` + `geemap`) that computes NDVI, NDBI, and LST for AMAC's wards, combined with the demographic and infrastructure layers above into one composite index. QA'd in QGIS, re-run on a schedule via GitHub Actions  so the ranking stays current without manual GEE or QGIS work each time.

## Repo structure

```
data/
  raw/          downloaded, never edited
  processed/    derived outputs
data-notes.md   full dataset documentation
project-brief.md   Week 1 brief
```

## Status

- **Week 1:** question drafted, data sources checked and linked, repository created.
- **Week 2:** real data downloaded and opened in QGIS (wards, health facilities, boundaries, roads), documented in `data-notes.md`. Known issue: OSM road extraction needs re-clipping to exact ward boundaries before it's usable for QA (see data-notes.md).

---
Andrew Jeremiah Ojonugwa
