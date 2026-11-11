# Decisions Log

Each entry: decision, reason, alternatives considered, status.

---

## D-001 — Initial study: rainfed corn, eastern/south-central Nebraska, 2018–2025
- **Date:** 2026-11-10
- **Decision:** Study rainfed (dryland) corn in ~20–30 eastern/south-central Nebraska counties over the 2018–2025 growing seasons.
- **Reasons:** Strong drought contrast within the Sentinel-2 era (severe 2022, continued 2023; wet 2019). Simple summer-crop phenology with a clear critical window (July–August, silking/grain fill). Irrigated corn in the same state can act as a natural control.
- **Alternatives considered:** Iowa corn (mostly rainfed, simpler masking, but weaker drought contrast and frequent summer cloud). Kansas winter wheat (strong drought, but crop abandonment biases harvested-area yields and phenology is complex).
- **Status:** Confirmed by user. Go/no-go check in Phase 2: confirm that NASS Quick Stats publishes county-level non-irrigated corn yields for Nebraska.

## D-002 — Python environment: uv, Python 3.12, pip wheels
- **Date:** 2026-11-10
- **Decision:** Manage the environment with `uv`, pinned to Python 3.12. Install rasterio/pyproj/geopandas from PyPI wheels (which bundle their own GDAL).
- **Reasons:** System default Python is 3.14, and geospatial wheel support often lags new Python releases. Bundled-GDAL wheels avoid conflicts with Homebrew GDAL 3.13.
- **Alternatives considered:** conda/mamba (heavier, not installed); Homebrew Python plus Homebrew GDAL bindings (version-coupling problems).
- **Status:** Proposed in Milestone 1.1.

## D-003 — Satellite access: STAC (Element84 Earth Search) as primary; Earth Engine optional
- **Date:** 2026-11-10
- **Decision:** Retrieve Sentinel-2 L2A through the Earth Search STAC API (cloud-optimised GeoTIFFs on AWS) and process with rasterio/xarray locally.
- **Reasons:** No account needed. Exposes real raster processing (CRS, windows, masking, scale/offset) instead of hiding it. Reproducible from code alone.
- **Alternatives considered:** Google Earth Engine (fast, but processing is server-side and opaque); Microsoft Planetary Computer STAC (good fallback); Copernicus Data Space (needs an account).
- **Status:** Proposed.

## D-003a — Reflectance conversion for Earth Search Sentinel-2 L2A (CORRECTED 2026-11-11)
- **Original decision (2026-11-10, WRONG):** Always apply `DN * scale + offset` using `raster:bands` metadata.
- **Corrected decision (2026-11-11):** Use `reflectance = DN * scale`. Apply the `raster:bands` offset **only if** `earthsearch:boa_offset_applied` is not True. On Earth Search, items with that flag set to True already have the +1000 baseline-04.00 offset removed from the pixel values, even though `raster:bands` still lists `offset: -0.1`.
- **How the error was found:** NDVI from `S2B_14TPL_20220721_0_L2A` came out at min/median/max = -7,838,315 / 1.51 / 1,060,320, which is physically impossible (NDVI must lie in [-1, 1]).
- **Evidence (decimated read of tile 14TPL, non-zero pixels, DN percentiles p1/p5/p50/p95):**
  - `S2B_14TPL_20220721_0_L2A` (baseline 04.00, flag True): red 212 / 236 / **444** / 1138; nir 1921 / 2477 / 3525 / 4738
  - `S2A_14TPL_20210728_0_L2A` (baseline 03.01, flag False, offset 0): red 231 / 257 / **388** / 1000
  - `S2A_14TPL_20210728_1_L2A` (baseline 05.00, flag True, offset -0.1): red 233 / 259 / **390** / 999
  - The same scene under the old and new baselines has nearly identical DNs, so the +1000 is not present in the pixels. A baseline-04.00 red median of 444 is also far below the ~1000+ it would be if the offset were still in the data.
- **Result after fix:** NDVI min/median/max = -0.29 / 0.78 / 0.94, which is plausible for mid-July cropland with rivers and lakes.
- **Open:** Rule for duplicate items (`_0` vs reprocessed `_1`). Likely prefer the newest processing baseline for consistency. To be decided in Phase 3.

## D-004 — Defer PostgreSQL/PostGIS
- **Date:** 2026-11-10
- **Decision:** Store processed outputs as Parquet/GeoParquet and COGs. Revisit a database in Phase 6 only if a concrete spatial query needs one.
- **Reasons:** The data volume is small (tens of counties × ~8 seasons) and results are precomputed.
- **Status:** Proposed.

## D-005 — US Drought Monitor is context, not ground truth
- **Date:** 2026-11-10
- **Decision:** Use USDM only as descriptive context. The independent reference is NASS county non-irrigated yield anomalies (detrended).
- **Reasons:** USDM blends expert judgement with indicators that may include vegetation data, so validating against it risks circularity.
- **Status:** Proposed.

## D-006 — Dryland mask approach (provisional)
- **Date:** 2026-11-10
- **Decision:** Intersect CDL corn pixels with a "rarely or never irrigated" mask derived from the LANID `irrFreq` layer (1997–2017).
- **Reasons:** LANID annual maps end in 2017. Centre-pivot irrigation infrastructure is fairly persistent.
- **Limitation:** Assumes irrigation status after 2017 matches its 1997–2017 history. Must be stated in the methodology and, if possible, checked.
- **Status:** Provisional; to be evaluated in Phase 2/3.
