# AgriScope — Mainpuri SAR Crop Mapping Dashboard

A responsive static dashboard for the Mainpuri Rabi 2025–26 crop-group classification workflow. The dashboard uses the supplied `data_exports.zip`, including the Level 1 and Level 2 GeoTIFFs, accuracy tables, area tables, confusion matrices, feature-importance exports, SAR signatures and NDVI signatures.

## Preview locally

1. Extract this package.
2. Open a terminal in the extracted `docs` folder.
3. Run:

   ```bash
   python -m http.server 8000
   ```

4. Open `http://localhost:8000` in your browser. Do not open `index.html` directly with `file://`; the dashboard fetches `data/dashboard.json`.

The Leaflet and Chart.js libraries and map basemaps load from external CDNs/services, so an internet connection is required for the full interactive experience.

## Publish with GitHub Pages

1. Create a repository, for example `mainpuri-sar-crop-mapping`.
2. Upload the contents of the `docs` folder to a top-level `docs/` directory in the repository.
3. In **Settings → Pages**, select **Deploy from a branch**, choose `main` and `/docs`, then save.
4. Wait for the deployment and open the Pages URL.

## Dashboard features

- Switch between Level 1 (three classes) and Level 2 (four classes).
- Interactive map with satellite/street basemaps, class legend, zoom, scale bar and opacity slider.
- Class-area charts, confusion matrix, class-wise producer's/user's accuracy and F1, validation metrics, VH/VV time series, NDVI signatures and feature importance.
- Responsive layout inspired by contemporary agriculture SaaS dashboards, while retaining the methodology and responsible-use context from the supplied master plan.

## Data handling notes

- Class ID `0` is treated as background/no-data and is excluded from crop-area charts. It is not presented as a second `Other` class.
- Accuracy is internal validation against reference labels derived from Sentinel-2 NDVI rules, not independent field-survey ground truth.
- “Potato-type/other early” is a crop-timing group and should not be read as a verified potato-only classification.
- The exported area percentage is recalculated among valid class-coded area so the displayed class shares add to approximately 100%.

Prepared for Barnali Bhowmick · M.Sc. Geoinformatics · BVIEER, Pune · 2026.
