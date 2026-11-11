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

## D-003a — Always read reflectance scale/offset from STAC metadata
- **Date:** 2026-11-10
- **Decision:** Convert digital numbers to reflectance as `DN * scale + offset`, taking scale and offset from each asset's `raster:bands` metadata. Never hard-code them.
- **Reasons:** Verified on Earth Search: offset is -0.1 for baseline ≥04.00 and 0 for older baselines. Ignoring it biases NDVI and EVI, especially in 2022 onwards (our drought year).
- **Open:** Rule for duplicate items (`_0` vs reprocessed `_1`) — likely prefer the newest processing baseline for consistency. To be decided in Phase 3.

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
