# Data Sources

Status key: **verified** = checked against provider documentation; **to verify** = believed correct, not yet checked.

| Dataset | Provider | Coverage | Temporal | Resolution | Access | Licence | Known limitations | Status |
|---|---|---|---|---|---|---|---|---|
| Sentinel-2 L2A surface reflectance | ESA Copernicus (via Element84 Earth Search) | Global land | 2015–present; ~5-day revisit since S2B (2017) | 10/20/60 m | STAC API `https://earth-search.aws.element84.com/v1`, COGs | Copernicus open data licence | Clouds/shadows. **Offset metadata is misleading:** baseline ≥04.00 items list `offset: -0.1` in `raster:bands`, but when `earthsearch:boa_offset_applied` is True the pixels already have the offset removed. Use `DN * 0.0001` and do not subtract it again (see D-003a for the evidence). **Duplicate items** exist for the same tile/date (e.g. `S2A_14TPL_20210728_0_L2A` at baseline 03.01 and `..._1_L2A` reprocessed at 05.00), so a de-duplication rule is needed. Study bbox crosses tile overlaps in UTM zones 14 and 15. | access verified 2026-11-10; offset handling corrected and verified against pixel values 2026-11-11 |
| Cropland Data Layer (CDL) | USDA NASS | CONUS | Annual, 2008–present | 30 m | CropScape / direct download | Public domain | Classification errors, especially at field edges; accuracy varies by state and crop | to verify |
| gridMET | Climatology Lab (Univ. of California, Merced) | CONUS | Daily, 1979–present | ~4 km | THREDDS / direct download | to verify | Modelled/interpolated, not station observations; coarse relative to fields | to verify |
| PRISM (alternative) | PRISM Group, Oregon State Univ. | CONUS | Daily, 1981–present | 4 km (800 m for some products) | Web download | to verify | Interpolated | to verify |
| NASS county corn yields (incl. irrigated / non-irrigated split) | USDA NASS Quick Stats | US counties | Annual | County | Quick Stats API | Public domain | Survey-based; suppressed counties; **2024 county estimates possibly missing** (discontinued April 2024, reinstatement announced March 2025) | **to verify — go/no-go** |
| LANID-US irrigation maps | Xie et al. 2021 (Zenodo DOI 10.5281/zenodo.5548555) | CONUS | Annual, 1997–2017 (+ `irrFreq` layer) | 30 m | Zenodo download | to verify | Ends 2017; reported >90% overall accuracy | coverage verified 2026-11-10 |
| US Drought Monitor | NDMC / USDA / NOAA | US | Weekly | Polygons (aggregated to county) | Web / API | to verify | Partly expert judgement; may include vegetation inputs — **context only, not ground truth** | to verify |
| NASS Crop Progress & Condition | USDA NASS | State level | Weekly, in season | State | Quick Stats | Public domain | Subjective ratings; state level only | to verify |
