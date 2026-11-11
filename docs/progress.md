# Progress

## Current status
- **Current phase:** 1 — GIS & Remote Sensing Foundations
- **Current milestone:** 1.1 — Reproducible environment and first remote raster read
- **Next action:** Step 6. Set up VS Code for notebooks, then create `notebooks/01_first_raster.ipynb` (first real Sentinel-2 image + NDVI).

## Milestone 1.1 steps
- [x] Step 1 — Install uv
- [x] Step 2 — Install Python 3.12
- [x] Step 3 — Create `pyproject.toml` + `.python-version`
- [x] Step 4 — Install tools and check they load
- [x] Step 5 — git init + first commit
- [ ] Step 6 — First notebook: open a Sentinel-2 image, compute NDVI
- [ ] Step 7 — Cross-check in QGIS; push to GitHub

## Completed
- 2026-11-10 — Environment audit: macOS 15.6.1 arm64; Python 3.14/3.13; GDAL 3.13.1; QGIS 4.0.3; Docker; PostgreSQL 15; Node 22; VS Code & Cursor.
- 2026-11-10 — Study selected and confirmed: rainfed Nebraska corn, 2018–2025 (see decisions D-001).
- 2026-11-10 — Initial docs created: decisions, research, data sources, progress.
- 2026-11-10 — uv 0.13.7 installed (Homebrew). Python 3.12.16 installed via uv and pinned in `.python-version`.
- 2026-11-10 — Project environment created with `uv` (`.venv`, `uv.lock`). Main deps: numpy, rasterio, pystac-client, geopandas, matplotlib. Dev deps: jupyterlab, ipykernel.
- 2026-11-10 — git repo started on branch `main`; first commit `dc3386a`.

## Verified outputs
- 2026-11-10 — Import check passed: `rasterio 1.5.2 | GDAL 3.12.2 | geopandas 1.2.0`. rasterio's bundled GDAL (3.12.2) is separate from Homebrew GDAL (3.13.1), as intended (see D-002).
- 2026-11-10 — `git status` before the first commit confirmed `.venv/` and data files are excluded by `.gitignore`.

## User setup
- Editor: VS Code
- GitHub username: iffat180 (repo visibility not yet decided)
- New to Jupyter notebooks; explain from scratch.
- Working style: explain each step and ask before creating or changing any file.

## Outstanding
- [ ] M1.1 Steps 6–7 (first Sentinel-2 read, NDVI, QGIS cross-check, GitHub push)
- [ ] Phase 2 go/no-go: confirm NASS county non-irrigated corn yields for Nebraska, and whether 2024 exists
- [ ] Choose specific counties
- [ ] Decide between gridMET and PRISM

## Blockers
None.

## Open questions for the user
- GitHub repo visibility (public or private)?
- Earth Engine account (optional, not needed for now)?
