# Research Record

## Research question (v1, 2026-11-10 — provisional until the Phase 2 data check)

For rainfed corn in ~20–30 eastern/south-central Nebraska counties over the 2018–2025 seasons, how much of the between-year and between-county variation in **NASS non-irrigated corn yield anomalies** (detrended) is explained by July–August Sentinel-2 **NDVI and EVI anomalies**, alone and combined with gridMET **precipitation, temperature and VPD anomalies**, evaluated with **leave-one-year-out cross-validation**?

- **Type of analysis:** Retrospective and explanatory. This is not yield forecasting. Yield anomaly is used only as an independent end-of-season reference for crop condition.
- **Unit of analysis:** County-year, restricted to dryland corn pixels.
- **Outcome:** Detrended NASS non-irrigated corn yield anomaly (bu/acre or %).

## Hypotheses
- **H1:** July–August vegetation-index anomalies are lower in dry seasons than in wet seasons.
- **H2:** NDVI and EVI anomalies differ in the strength of their relationship with yield anomalies (NDVI may saturate over dense corn canopy).
- **H3:** Adding weather anomalies (precipitation, Tmax, VPD) improves out-of-sample explanatory skill over vegetation indices alone.
- **Control check:** The relationships in H1/H3 are weaker in irrigated corn.

## Planned methods
- **Baseline:** Climatology / trend-only yield model (predicts the trend yield, with zero anomaly).
- **Comparisons:** NDVI-only vs EVI-only vs VI + weather, using simple, regularised linear models.
- **Validation:** Leave-one-year-out (temporal holdout). Spatial holdout as a secondary check.
- **Metrics:** Out-of-sample RMSE / MAE of the yield anomaly; correlation, with bootstrap confidence intervals. No classification metrics, because there is no classification target.

## Known limitations (initial)
- Only ~7–8 seasons, so the effective sample size for interannual variation is close to the number of years.
- The 2024 county yields may be missing (NASS discontinued county estimates for the 2024 crop year; reinstatement was announced in March 2025. Needs verification).
- The dryland mask assumes irrigation status after 2017 is stable (see D-006).
- County yields are survey-based estimates with their own error, and some counties are suppressed ("Not Published").
- Correlation does not establish drought causation; yield also depends on hail, disease, management, planting date and hybrids.

## Literature
To be compiled in Phase 2. Only sources that have been read and verified are listed.
- Xie, Y. et al. (2021). LANID-US: Landsat-based Irrigation Dataset for the United States. *Earth System Science Data* 13, 5689–5710. https://essd.copernicus.org/articles/13/5689/2021/

## Findings
None yet.
