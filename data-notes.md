[data-notes.md](https://github.com/user-attachments/files/32173871/data-notes.md)
# Data notes

## GRID3 NGA  Operational Wards v3.0
- Source: https://data.grid3.org
- Citation: CIESIN, Columbia University. 2026. GRID3 NGA  Operational Wards v3.0. https://doi.org/10.7916/2pfj-hq78
- Downloaded: [date]
- 12 features, polygons
- Columns: fid, country, iso3, state, statecode, lga, lga_alt_na, ward, ward_alt_n, ward_v1_gr, ward_in_gr, multipart_, source, date, area_sqkm
- Nulls: ward_alt_n is null for wards with no alternate name (Gui, Gwagwa, Jiwa, Kabusa, Karu, Orozo); ward_v1_gr is null throughout
- Covers: FCT Abuja, AMAC's 12 wards, City Center 1, Garki 1, Gui, Gwagwa, Gwarinpa, Jiwa, Kabusa, Karshi 1, Karu, Nyanya 1, Orozo, Wuse
- Internal dataset date field: 2026-06-30
- Quality note: operational-use boundaries, not yet fully validated by government authorities (per GRID3's own documentation)

## AMAC Boundary (reference layer for clipping/verification)
- Source: GADM v4.1, https://gadm.org  via gadm41_NGA_shp
- Downloaded: [date]
- 1 feature (Abuja Municipal, Local Authority), polygon, single dissolved LGA outline
- Also referenced: gadm41_NGA_shp state-level layer, 37 features (36 states + FCT), used only to isolate FCT before clipping to AMAC
- Role: not fed into the index directly, used to sanity-check that the 12 GRID3 wards above sum to the same overall AMAC extent as an independent boundary source

## GRID3 NGA - Health Facilities v3.0
- Source: https://data.grid3.org
- Citation: CIESIN, Columbia University. 2026. GRID3 NGA - Health Facilities v3.0. https://doi.org/10.7916/s6cq-1q98
- Downloaded: [date]
- 252 features, points
- Columns: fid, unique_id, latitude, longitude, country, iso, state_stan, lga_standa, ward_stand, ward_bdry, ward_in_gr, facility_n, alt_name, settlement, facility_l, facility_t, facility_o, facility_1, functional, date_creat, sett_ext_t, mgrs_code, input_data, input_da_1, nhfr_facil, gps_accura, sett_ext_d, dist_ward_, flag1–flag6, issues, flag_count
- Nulls: alt_name and settlement are null for nearly every row; date_creat is null except one record; nhfr_facil is null throughout; issues is null except a handful of flagged rows
- Quality note: dataset is explicitly non-exhaustive and non-validated per GRID3's documentation — a ward showing zero facilities here may reflect a genuine gap or a data gap, not confirmed absence
- Bonus: the dataset ships its own QA columns (flag1–flag6, issues, flag_count) e.g. two records are flagged "cluster with another" and one "outside of ward." Worth filtering on flag_count before feeding facility counts into the index, rather than treating all 252 rows as equally reliable.

## OSM roads/highways, extracted via OSM Extractor (Jambar Lab)  QA cross-check only
- Query: highway=* within AMAC extent
- Extracted: [date]
- 60,563 features, lines
- Columns: standard OSM tag schema — highway, surface, name, oneway, maxspeed, lanes, bridge, layer, waterway, amenity, and many sparsely-populated tag columns (driveway, wheelchair, brand, atm, etc.  nearly all null, which is normal for OSM's tag-per-object model)
- **Coverage flag — check before using:** 60,563 features is roughly 50x the count the pack's own worked example got for a comparable single LGA (1,247 for Ibadan North). The map view also shows road/built-up coverage extending well past the red AMAC ward outline into surrounding areas. This suggests the extraction pulled a bounding-box extent rather than clipping precisely to the 12 ward polygons. Re-run with an exact polygon clip before this feeds the QA check — otherwise it'll flag false "gaps" or mask real ones by comparing against roads that aren't actually inside AMAC.
- Role: QA only, not fed into the index (GRID3 Roads v1.0 remains primary per our earlier decision)

That last line about coverage is the kind of note that saves you in month six.

Commit it with the message `Add data notes`.

Keep adding to this file all year.
