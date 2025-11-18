# Raster Tile Mosaicing

This repository contains my solution for the **Raster Tile Mosaicing** task.
The goal is to create a **single cloudless mosaic GeoTIFF** from multiple adjacent satellite raster tiles, with consistent CRS, resolution, and proper handling of NoData and overlaps. All processing is demonstrated in a **Google Colab notebook** and is fully reproducible.

---

## 1. Environment & Requirements

The notebook is designed to run directly on **Google Colab**, so no local installation is strictly required.

Main Python libraries used:
- `rasterio`
- `numpy`
- `matplotlib`
- `glob`
- `json`
- `tqdm` (optional, for progress bars)

In Colab the notebook installs required packages inside the runtime (first code cell runs `pip install` as needed). A minimal `requirements.txt` is included next to the notebook for local installs: `requirements.txt`.

---

## 2. Data

The dataset of adjacent GeoTIFF tiles is **not stored in this repo**. The notebook automatically downloads the sample data from the official task URL at runtime:

- Dataset URL: `https://objectstore.e2enetworks.net/btechtasksampledata/data.zip`

This keeps the repo light. When you run the notebook (Colab / Kaggle / local), the data is downloaded into a `data/` folder.

---

## 3. How to Run (Google Colab)

1. Open the Colab notebook `A4AI.ipynb` from this folder (or upload it to Colab).
2. Set runtime: **Runtime → Change runtime type → Python 3** (GPU not required).
3. Run the notebook cells from top to bottom:
   - Cell 1 installs dependencies and sets paths.
   - An early cell downloads and unzips `data.zip` into `data/`.
4. After completion you will find outputs in the working directory (or `/content/` on Colab):
   - `cloudless_mosaic.tif`
   - `cloudless_mosaic_cloudmasked.tif` (optional)
   - `cloudless_mosaic_provenance.json`
   - Inline visualizations and QA stats in the notebook.

To install dependencies locally using the included `requirements.txt`:

```bash
pip install -r requirements.txt
```

---

## 4. Outputs

- ✅ `cloudless_mosaic.tif` – final georeferenced mosaic
- ✅ (Optional) `cloudless_mosaic_cloudmasked.tif` – mosaic after simple cloud masking / NoData handling
- ✅ `cloudless_mosaic_provenance.json` – provenance metadata (inputs, CRS, resolution, steps)
- ✅ Visualization previews and basic QA statistics (NoData counts, metadata dump)

---

## 5. Cell-by-Cell Overview (logical order)

1. Setup & Imports — install (if on Colab) and import `rasterio`, `numpy`, `matplotlib`, etc.
2. Path Configuration — set working dir, `data/`, and output paths.
3. Download & Unzip Dataset — download `data.zip` and extract into `data/`.
4. Discover Input Tiles — `glob` for `.tif` files and print counts/sample.
5. Validate Tile Metadata — read tile CRS, resolution, and bounds; ensure consistency.
6. Mosaic Creation — use `rasterio.merge.merge` to combine tiles into a mosaic.
7. Save Mosaic GeoTIFF — write `cloudless_mosaic.tif` with correct profile and NoData.
8. Metadata Validation — re-open mosaic to print CRS, transform, resolution, dimensions, dtypes, NoData.
9. Visualization — plot RGB or downsampled preview with `matplotlib`.
10. Quality Metrics — compute NoData pixel counts and percentages.
11. Provenance JSON — write `cloudless_mosaic_provenance.json` with input tiles and processing steps.
12. Quick Demo — merge 1–2 tiles for a fast preview.
13. Low-Res Preview / PNG Export (optional) — downsample and save a small PNG for reports.
14. Final Display — show final mosaic to confirm success.

---

## 6. Assignment Checklist

- [x] Python-based mosaicing workflow using `rasterio` and `numpy`.
- [x] Proper handling of spatial reference, alignment, and NoData.
- [x] Georeferenced raster output with CRS and transform saved.
- [x] End-to-end demonstration in a single Colab notebook (install, download, process, save).

---

## 7. Notes & Tips

- The notebook is written for Colab; when running locally, ensure `rasterio` and its native dependencies are installed. Using `conda` can simplify geospatial package installation on macOS.
- If tiles differ in CRS/resolution, reproject/resample to a common grid before merging.
- The simple cloud mask in the notebook uses an RGB brightness threshold; for production use, prefer cloud-specific detection methods (e.g., QA bands or ML-based masks).

---

## 8. Files in this folder

- `A4AI.ipynb` — the Colab-ready notebook implementing the full workflow.
- `requirements.txt` — minimal package list for local installs (`rasterio`, `numpy`, `matplotlib`).
- `README.md` — this file.

---

If you want, I can:
- Add a short `How to run locally` section with `conda` instructions for macOS.
- Generate a small low-resolution PNG preview saved by the notebook for quick viewing.
- Tweak wording or add a one-line abstract for a report header.

Which of these would you like next?