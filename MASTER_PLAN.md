# MASTER PLAN 
**Mainpuri · Rabi 2025-26 · Sentinel-1 · Random Forest · GitHub Pages dashboard**
---

## PART 0 — The 2-minute summary

**Your real problem:** you cannot tell wheat from mustard from potato by looking at satellite pictures, so you cannot draw training points by hand.

**The fix:** do NOT draw training points by hand. Let the computer make them from a *different* satellite (Sentinel-2, the ordinary "camera" satellite). Every crop has a "greenness calendar":

| Crop group | When is it green? | What the computer looks for |
|---|---|---|
| **Wheat-type** | slow in December, strong in February, **still green in late March** | green late |
| **Early-harvest crops** (mustard / potato) | green in **December–January**, **harvested before late March** | green early, gone by late March |
| **Other** | never green (fallow/bare) or **always** green (trees) | flat line |

Sentinel-2 greenness → automatic labels → radar (Sentinel-1) learns the labels → radar makes the full map. Two different satellites, so it is not circular.

**Why this is allowed:** your task sheet (§5) says a *"carefully justified crop/non-crop or crop-group classification may be accepted when detailed crop labels cannot be validated reliably"*, and (§10) that if independent validation is not possible you must call the result an *"internal or provisional assessment"*. We do exactly that, honestly.

**Your first three actions right now**
1. Create the Earth Engine asset folder `mainpuri_sar` and put your path in `ASSET_ROOT` in both scripts.
2. Paste `FT1_reference_labels.js` into the GEE Code Editor and run it (Level 1).
3. Open the NDVI curve chart it prints. Do the three curves look like the table above? If yes, you are on track.

**What I will NOT help with:** inventing field data or fake "ground truth". The plan is built so you never need it.

---

## PART 1 — Why the plan changed (plain language)

* **Old plan:** you click on fields, say "this is wheat", and the computer learns from your clicks. It needs expert eyes. Radar images look like grey noise, and even optical images are hard if you have never seen these crops.
* **New plan:** crops are identified by **timing** (when green, when harvested), which a computer reads from numbers — no eyes needed. You only check that the *curves* behave.
* **What you give up:** the reference is "optical-derived", not "field-surveyed". So accuracy must be reported as **provisional/internal**. You also report **crop groups** (wheat-type / early-harvest) rather than naming potato and mustard with certainty. Level 2 tries to split mustard using its yellow flowers.
* **What you gain:** a complete, honest, reproducible workflow that satisfies the sheet and can be finished in one evening.

---

## PART 2 — The task sheet turned into an ordered to-do list

Order = the order you do them. **S** = done by the scripts, **Y** = you do it, **D** = a document you write.

### A. Before analysis (sheet §6, §7, §8)
| # | Task | Who | Time | Notes |
|---|---|---|---|---|
| A1 | Read the 5 mandatory papers (abstract → study-area figure → methods → accuracy table → conclusion) | Y | 60–75 min | Do it **while GEE tasks run** (see Part 5). Needed for the paper-review matrix |
| A2 | Watch the 2 videos (speckle filtering; crop-type detection) at 1.5× | Y | 30 min | Take 5 bullet notes each; the viva may ask |
| A3 | Mini-proposal (one page) | D | 20 min | Pre-written text in Part 7 |
| A4 | (Sheet §14) Feasibility message to coordinator | Y | 5 min | Draft in Part 7. Your area has small fields and no ground data, which the sheet says to discuss early |

### B. The 21 steps of sheet §9 mapped to tonight
| § 9 step | What the sheet wants | Tonight |
|---|---|---|
| 1 Research question | what crops, which season, why here | **D** — in mini-proposal |
| 2 Crop calendar | sowing→harvest per crop | **D** — table in dashboard + Part 7; cite ICAR-CRIDA Mainpuri plan / KVK |
| 3 Area of interest | valid boundary, manageable size | **S/Y** — rectangle (or village boundary if you already saved `aoi_final`) |
| 4 Class scheme | classes you can distinguish and verify | **Y** — Level 1 (3 classes); Level 2 optional |
| 5 Reference information | credible, dated, with provenance | **S** — FT1 automatic labels; **D** — describe provenance honestly |
| 6 Select SAR time series | sensor, dates, orbit, polarisation | **S** — FT2 picks one orbit automatically |
| 7 Metadata / completeness | missing dates, orbit problems | **S** — FT2 prints dates and per-window gap filling |
| 8 Preprocess | noise, calibration, terrain, units, speckle | **S** — GEE already did the first four; FT2 does dB→linear + Quegan filter |
| 9 Temporal composites | regular intervals | **S** — 16 windows × 12 days |
| 10 Predictor variables | VV, VH, ratios, statistics | **S** — 44 features |
| 11 Temporal signatures | plot class curves | **S** — FT2 charts + CSV |
| 12 Training/validation design | independent, no leakage | **S** — split by 500 m blocks, 70/30, 5 repeats |
| 13 Train classifier | justified, parameters documented | **S** — Random Forest, 300 trees, seed 42 |
| 14 Variable behaviour | feature importance | **S** — importance bar chart |
| 15 Classify study area | the map | **S** |
| 16 Post-processing | only if defensible | **S** — 3×3 majority filter; both raw and smoothed accuracy reported |
| 17 Accuracy | confusion matrix, OA, PA, UA, F1 | **S** — CSVs for dashboard |
| 18 Crop area | by class, units, uncertainty note | **S** — hectares; **D** — uncertainty sentence in Part 4.7 |
| 19 Uncertainty/limitations | mixed pixels, moisture, etc. | **D** — dashboard "Limits" section is pre-written |
| 20 Publish | webpage, repo, README, test | **Y** — Part 6 |
| (sheet §6) | paper-review matrix | **D** — Part 7 |
| (sheet §12 #10) | AI-use declaration | **D** — Part 7 |

---

## PART 3 — Submission details and required outputs

**Facts from the sheet:** Individual assignment · Coordinator **Dr. Aravinth R** · Date of issue is blank · **Submission and viva date: to be announced** (confirm with coordinator) · Viva **10 min, max 15** · Classifier and software are your choice if justified.

### The 10 deliverables (sheet §12)
| # | Deliverable | Minimum requirement | Where it comes from | ✔ |
|---|---|---|---|---|
| 1 | Public GitHub repository | organised files, assets, scripts/workflow notes, complete README | this pack + README (Part 7) | ☐ |
| 2 | GitHub Pages URL | working public page, tested in browser **and mobile-size screen** | Part 6 | ☐ |
| 3 | Study-area package | boundary, location map, crop calendar, class definitions, reference-data description | AOI file + dashboard "Study area" + Part 7 text | ☐ |
| 4 | SAR workflow record | sensor/acquisition details, preprocessing sequence, temporal features, classifier settings | FT2 console prints + README table (Part 7) | ☐ |
| 5 | Final crop map | legible map with legend, location context, date/season, source | dashboard map + PNG export | ☐ |
| 6 | Validation results | independent confusion matrix, OA, PA, UA, F1, **sample counts and sampling explanation** | `confusion_matrix.csv`, `classwise_accuracy.csv`, `accuracy_summary.csv` | ☐ |
| 7 | Crop-area summary | area by class, units, uncertainty note | `area_by_class.csv` | ☐ |
| 8 | Interpretive visuals | at least one temporal-signature plot + other public-facing visual | `sar_signatures.csv`, `ndvi_signatures.csv` → dashboard charts | ☐ |
| 9 | Paper-review matrix | all 5 papers + how literature influenced the project | Part 7 | ☐ |
| 10 | AI-use declaration | whether/how AI was used and what **you** verified | Part 7 | ☐ |

### Webpage minimum content (sheet §11) → where it is in the dashboard
| Required item | Section id in `index.html` |
|---|---|
| plain-language title + overview | `#overview` |
| non-technical SAR / backscatter / multi-temporal explanation | `#sar` |
| study-area location, agricultural context, season, crop calendar | `#area` |
| data sources, acquisition dates, polarisations, processing | `#data` (dates: paste the date list printed by FT2) |
| interactive map: legend, scale, north, layer controls, basemap attribution, credits | `#mapsec` |
| training and validation method | `#data` + `#accuracy` |
| confusion matrix + labelled accuracy measures | `#accuracy` |
| class-wise area statistics | `#areas` |
| ≥1 temporal-signature graph | `#signatures` |
| limitations, uncertainty, responsible use | `#limits` |
| student name, programme, subject, year, repo link, references | footer + `#refs` — **edit repo link** |

### Accuracy rules (sheet §10) — do not break these
1. Report a confusion matrix, **OA, PA, UA, F1 per class** (kappa only if justified).
2. State the **number and distribution** of training and validation samples.
3. Say how samples were split (random / spatial / by field) and how leakage was prevented → *"split by 500 m spatial blocks; whole blocks held out"*.
4. If validation is not fully independent, say so **prominently** and call it **internal / provisional**. (The dashboard already has this warning box.)
5. Never present training accuracy as map accuracy.
6. Explain which classes are confused and why (phenology, small fields, moisture…).

---

## PART 4 — The methodology (explained simply)

### 4.1 The whole pipeline in one picture
```
 Sentinel-2 (optical)                         Sentinel-1 (radar)
 greenness Nov→Apr per pixel                  echo strength VV, VH every 12 days
        │                                              │
        ▼  FT1: rules from crop calendar               ▼  FT2: clean + filter + 16 windows
 AUTOMATIC LABELS  ──────────────►  learn ◄──────  44 radar features per pixel
 (wheat-type / early-harvest / other)   Random Forest
                                          │
                       classify every pixel → CROP MAP → test on held-out blocks
                                          │
                  CSV tables + GeoTIFF → build_site.py → GitHub Pages dashboard
```

### 4.2 STEP A — make the labels (script `FT1_reference_labels.js`)
**What it does, like a teacher with a rule book:** for each 10 m pixel it measures greenness (NDVI) in six periods (Nov, Dec, Jan, Feb, early Mar, late Mar). Fog hides some months, so missing months are filled using neighbouring months.

**The rule book (the numbers you may tune, object `T`):**
| Label | Rule | Plain meaning |
|---|---|---|
| Wheat-type (1) | late-March NDVI ≥ 0.50 **and** Feb ≥ 0.50 **and** Dec ≤ 0.55 | green late, not already lush in December |
| Early-harvest (2) | max(Dec, Jan) ≥ 0.55 **and** late-March ≤ 0.35 | green early, gone by late March |
| Other (3) | never above 0.35 (fallow/bare) **or** ≥ 0.60 in every period (trees) | flat line |
| ignored | everything in between | ambiguous → **not used for training** (this is on purpose) |

Then only **pure pixels** are kept: a pixel must have all 8 neighbours with the same label, which removes field edges and mixed pixels. Buildings and water (ESA WorldCover) are excluded.

**How to check the result — no crop expertise needed:**
1. The script prints **"Average NDVI curve of each reference class"** (x: 1=Nov … 6=late March).
   * Wheat-type: rises and is **high at 5–6**.
   * Early-harvest: **high at 2–3**, **falls to low at 5–6**.
   * Other: a flat low or flat high line.
2. On the map, labels should form **field-shaped patches**, not salt-and-pepper dots.
3. Turn on the layer *"S2 false colour late Mar"*: in this layer, still-green crops are **bright red**. Wheat-type patches should be red; early-harvest patches should not be.
4. Optional cross-check: layer *"WorldCereal 2021 winter cereals"* — a different year, so only expect the *broad* pattern of cereal areas to agree.

**If the curves look wrong — fix table**
| What you see | Likely cause | Change in `T` |
|---|---|---|
| Almost no early-harvest pixels | haze lowered Dec/Jan NDVI | `early_peak` 0.55 → 0.45 |
| Early-harvest curve not falling by Mar | `early_late` too loose | `early_late` 0.35 → 0.30 |
| Almost no wheat-type pixels | late March NDVI lower than expected | `wheat_late` 0.50 → 0.42, `wheat_feb` 0.50 → 0.42 |
| Wheat curve already high in Dec | catching early crops | `wheat_dec` 0.55 → 0.45 |
| One class has < 200 pixels | thresholds too strict / small area | loosen the relevant threshold; or `PURE_RADIUS` stays 1 |
| Everything is "Other" | NDVI layer is empty (clouds/fog) | check S2 layers load; widen `PERIODS` dates by ±7 days |

**Rule for honesty:** tune thresholds by looking at the **NDVI curves and map shapes in FT1 only** — never by looking at FT2 accuracy. Write down every change in your log.

When satisfied: set `EXPORT = true`, run, go to the **Tasks** tab, click **Run** on `ref_labels` and `ndvi_signatures`. Wait for the task (usually a few minutes). **Use that waiting time for Part 5 paper reading.**

### 4.3 STEP B — radar classification (script `FT2_sar_classify_validate.js`)
Run once the asset `ref_labels` exists. What the script does, in plain words:
1. **Chooses the orbit** with the most full-coverage images and sticks to that single track (same viewing angle every time → fair comparison). It prints the choice and all dates (copy the dates into your README).
2. **Cleans the radar:** removes extremely dark noise pixels, converts dB → real power (averaging must be done in power, not dB), applies the **Quegan multi-temporal speckle filter** (*speckle* = grainy noise of radar; the filter uses all dates together so fields stay sharp).
3. **Builds 16 time windows of 12 days** (16 Oct → ~26 Apr). Empty windows are filled from neighbours. Averaged in power, converted back to dB.
4. **Makes 44 features per pixel:** VV and VH for each window (32), plus season-long mean/spread/min/max/range for VV and VH (10), VV–VH difference and Radar Vegetation Index (2).
5. **Takes training samples** from the FT1 labels (up to 800 pixels per class — this stops wheat swamping the rest) and **splits them by 500 m blocks** into 70% train / 30% validation. Whole blocks go to one side only → neighbouring pixels of the same field cannot leak.
6. **Trains Random Forest** and **tests on the held-out blocks:** confusion matrix, OA, PA, UA, F1.
7. Repeats the split with **5 seeds** (OA mean ± SD) — proves the number is not a lucky draw.
8. Draws **feature importance** (which radar variables mattered).
9. **Classifies the whole area**, masks buildings/water, makes a 3×3 majority-smoothed version, computes **area per class (hectares)** and **radar signatures per class**.
10. With `EXPORT = true` writes: `confusion_matrix.csv`, `classwise_accuracy.csv`, `accuracy_summary.csv`, `area_by_class.csv`, `sar_signatures.csv`, `crop_map_wgs84.tif`, `crop_map_utm44n.tif` to Drive folder `mainpuri_sar_ft`.

### 4.4 Reaching more than 75% — what to expect and what you may do
Because the labels are made from "clear-cut" pixels, radar usually separates them quite well, so your target is realistic. **I cannot promise a number before running it.** If the first run is below 75%, apply these levers **in this order, one at a time**, and write down each result:

| Order | Lever | Where | Why it helps |
|---|---|---|---|
| 1 | Look at the confusion matrix: which two classes mix? | FT2 console | tells you the real problem |
| 2 | Raise `PER_CLASS` 800 → 1500 | FT2 | more training pixels |
| 3 | Re-check FT1 curves; tighten thresholds so labels are cleaner | FT1 | noisy labels cap accuracy |
| 4 | Make sure `FILTER = 'quegan'` | FT2 | less speckle |
| 5 | Report the 3×3-smoothed map accuracy too | FT2 prints it | removes salt-and-pepper |
| 6 | If Level 2: go back to Level 1 (merge mustard + potato-type) | both | the two early classes look alike in radar |

**Not allowed (and examiners can spot it):** choosing the seed that gives the best accuracy, moving validation pixels after seeing errors, deleting a hard class without saying so, hiding the first run. Report the final result **and** mention the earlier one.

### 4.5 Level 1 or Level 2?
* **Do Level 1 first and finish the whole chain** (labels → map → CSVs → dashboard). That is your safe submission.
* **Only if you have 45 spare minutes**, run Level 2 (`LEVEL = 2` in both scripts). FT1 will find the yellow-flower split automatically (Otsu threshold on the yellow index NDYI) and print a **separation score**: above ~0.5 = two clear groups; below = warning, do not trust it. Check by eye: switch on *"S2 true colour Jan–Feb"* — mustard fields look **bright yellow**; they should match the Mustard-type class. If not, stay with Level 1.
* If you switch to Level 2 after exporting Level 1, re-export `ref_labels` and re-run FT2, then run `build_site.py --level 2`.

### 4.6 How to talk about it honestly (copy-paste)
* **Reference data:** *"Field-surveyed crop labels were not available for the study area. Reference labels were therefore derived automatically from Sentinel-2 NDVI time series using crop-calendar rules (late-season cereal vs early-harvest crops vs other), retaining only spatially homogeneous pixels. Classes are crop groups defined by phenology. The accuracy reported is an internal/provisional assessment against this optical-derived reference."*
* **Why the accuracy may look high:** *"The reference contains only confident, homogeneous pixels; the true accuracy for ambiguous pixels and against field-surveyed data is expected to be lower."*
* **Independence:** *"Training and validation samples were separated by 500 m spatial blocks, so no block contributed pixels to both sets; the split was repeated with five seeds."*

### 4.7 Area uncertainty sentence (sheet §9 step 18)
*"Areas are obtained by counting 10 m pixels of the smoothed map and therefore inherit classification error; classes with lower producer's or user's accuracy are over- or under-estimated, and small classes are affected most. Error-adjusted areas were not estimated because no independent probability sample of reference points was available."*

---

## PART 5 — Time plan

**Assumption:** about **10 hours** available. I do not know your clock, so times are "hours from now". Tasks marked ⏳ are Earth Engine jobs that run on Google's servers; **work on a ‖ parallel task** while they run.

| Clock | Task | Output | ‖ Parallel while waiting |
|---|---|---|---|
| 0:00–0:20 | Set up: asset folder, edit `ASSET_ROOT`, GitHub account/repo created, folder `docs/` ready | ready | |
| 0:20–1:10 | **FT1** run, read curves, tune `T`, export labels ⏳ | `ref_labels` asset | start **A1** (papers 1–2) |
| 1:10–2:40 | **FT2** run, read orbit/dates/counts/accuracy; apply levers if < 75% | accuracy ≥ target | **A1** papers 3–5 |
| 2:40–3:10 | FT2 with `EXPORT = true`, run all 7 tasks ⏳ | files in Drive | start paper-review matrix |
| 3:10–3:55 | *Optional* Level 2 attempt (hard stop at 45 min) | keep only if clear | |
| 3:55–4:40 | Download exports, run `build_site.py`, preview locally (`python -m http.server`) | `docs/` complete | |
| 4:40–5:40 | Edit dashboard text (repo link, orbit, dates, calendar source); publish on GitHub Pages; test on phone-size screen | **public URL** | |
| 5:40–7:10 | Documents: mini-proposal, README (workflow record), study-area package, AI-use declaration, finish paper matrix | docs | |
| 7:10–8:10 | Check every item in Part 3; fix gaps; screenshots; re-test URL | checklist all ✔ | |
| 8:10–9:00 | Viva prep (Part 9): read your own dashboard aloud | confident | |
| 9:00–10:00 | **Buffer** (something always breaks) | | |

**If you only have 6 hours:** skip Level 2; do A2 videos at 2× speed; reduce documents to short versions; keep buffer at 30 min.
**If you only have 3 hours:** priority order → (1) FT1 + FT2 Level 1 and exports, (2) dashboard published, (3) accuracy tables on the page, (4) README + AI declaration, (5) paper matrix (even brief), (6) mini-proposal. A published page with honest limits beats a perfect map that is not online.

**Stop rules:** if one step takes 2× its slot, stop and use the fallback in Part 8. Never skip the GitHub Pages publish.

---

## PART 6 — Dashboard plan (files are ready)

**Pack layout**
```
site/
  build_site.py          # turns Drive exports into website data
  docs/
    index.html           # the dashboard (all required sections already written)
    data/ , assets/      # filled by build_site.py
```
**Steps**
1. Download the Drive folder `mainpuri_sar_ft` (7 files). Put them in one local folder.
2. `pip install numpy pandas rasterio pillow` (if `rasterio` fails on your PC, run the script in **Google Colab**: upload the exports and the script, same pip line).
3. `python build_site.py --exports "path/to/mainpuri_sar_ft" --level 1` (use `--level 2` if you did Level 2).
4. Preview: `cd docs` then `python -m http.server 8000` → open `http://localhost:8000`. (Double-clicking the HTML will not load the data.)
5. Edit `index.html`: (a) footer: your repo link; (b) section `#data`: paste the real list of acquisition dates; (c) section `#area`: add one sentence about your boundary; (d) check your name/programme line.
6. Make a GitHub repo `mainpuri-sar-crop-mapping`. Upload everything (drag the folders into the web page, or use git).
7. **Settings → Pages → Build and deployment → Deploy from a branch → `main` → folder `/docs` → Save.** Wait ~1–2 minutes; your URL is `https://<username>.github.io/mainpuri-sar-crop-mapping/`.
8. Test: desktop browser; then press **F12 → device toolbar** to check a phone-size screen; confirm map, slider, charts and tables show.

**What the dashboard already includes:** plain-language overview and SAR explanation, study-area and crop-calendar table, data/method boxes, Leaflet map with legend, scale bar, north arrow, layer switcher, opacity slider, attribution, confusion matrix, PA/UA/F1 table, sample counts, area table + chart, VH/VV toggle chart, NDVI chart, limitations/responsible use, references, footer. Colours are colour-blind-safe (Okabe–Ito), the page is responsive and supports dark mode.

**Fallback if Python will not run:** in GEE, after FT2, run
`print(mapSmooth.visualize({min:1, max:CLASSES.length, palette:PAL}).getThumbURL({region: aoi, dimensions: 1400, format: 'png', crs: 'EPSG:3857'}));`
open the printed link, save the PNG as `docs/assets/cropmap.png`, and write `docs/assets/cropmap_bounds.json` as `[[27.23,79.075],[27.285,79.15]]` (south-west and north-east corners of your AOI; use your own numbers if you changed the area). You then have to type the table values into `data/data.json` by hand — ask me and I will do that conversion if you paste the CSVs.

---

## PART 7 — Documents to write (ready-to-paste text)

### 7.1 Mini-proposal (one page)
* **Study area:** farming block near Sultanganj, Mainpuri district, Uttar Pradesh; working window 27.23–27.285°N, 79.075–79.15°E (≈ 45 km²). *(Boundary source: research window / Census-2011 village polygons if you did that step.)*
* **Season and year:** Rabi 2025-26, 15 Oct 2025 – 26 Apr 2026.
* **Expected crops:** wheat, mustard, potato (plus fallow, trees, other).
* **Crop calendar:** table in dashboard `#area` (cite ICAR-CRIDA Mainpuri contingency plan / KVK).
* **Research question:** *Can multi-temporal Sentinel-1 backscatter separate winter crop groups (late-season cereal vs early-harvest crops) in a small, fragmented Indo-Gangetic farming landscape, and how reliable is the result when reference data come from optical phenology?*
* **SAR dataset:** Sentinel-1 GRD IW, VV+VH, single relative orbit, 12-day windows (Google Earth Engine).
* **Provisional classes:** Wheat-type, Early-harvest crops (mustard/potato), Other; optional split of mustard-type.
* **Reference data:** automatic labels from Sentinel-2 NDVI phenology rules; no field survey.
* **Classifier:** Random Forest (300 trees).
* **Limitations:** no ground truth, small fields/mixed pixels, similar phenology of mustard and potato, moisture effects, internal validation only.

### 7.2 Message to coordinator (sheet §14 asks for early discussion)
> Respected Sir, for my SAR crop-mapping assignment (Mainpuri, Rabi 2025-26) I do not have field-surveyed crop data and the fields are small. I plan to derive reference labels from Sentinel-2 NDVI phenology rules and map crop groups (late-season cereal / early-harvest crops / other), reporting accuracy as an internal/provisional assessment as per §10 of the task sheet. Kindly let me know if this approach is acceptable.

### 7.3 README skeleton (= SAR workflow record, deliverable 4)
| Item | Fill with |
|---|---|
| Sensor / product | Sentinel-1 GRD IW, VV+VH, 10 m (COPERNICUS/S1_GRD) |
| Orbit | pass + relative orbit printed by FT2 |
| Dates | list printed by FT2 (first–last date, number of scenes) |
| Preprocessing done by GEE | orbit file, thermal & border-noise removal, calibration, terrain correction, dB |
| Preprocessing by me | noise-floor mask (VV > −30 dB, VH > −35 dB), dB→linear, Quegan multi-temporal filter (3×3), 12-day mean in power, gap-fill |
| Windows | 16 × 12 d from 2025-10-16 |
| Features | 44 (list) |
| Reference | Sentinel-2 NDVI rules (thresholds `T`), pure pixels, WorldCover mask |
| Split | 500 m blocks, 70/30, seed 42, 5 repeats |
| Classifier | RF 300 trees, bag fraction 0.5, seed 42 |
| Post-processing | 3×3 majority |
| Results | OA, PA, UA, F1, area |
| Reproduce | run FT1 → FT2 → `build_site.py` |
Also include: data credits (Copernicus Sentinel-1/2, ESA WorldCover, Google Earth Engine), licence, your name.

### 7.4 Paper-review matrix (deliverable 9)
Make a table with **5 rows (papers) × 9 columns** (the sheet's headings): study area/context · classes · SAR sensor/band/polarisation/orbit/dates · preprocessing & speckle · inputs/temporal features · classifier & training strategy · validation & reported accuracy · main finding/limitation/relevance · **one thing I adopt / modify / avoid**.
**Fast reading route (≈ 12 min per paper):** abstract → study-area figure → methods table → accuracy table → conclusion; copy facts only from the paper.
**How each paper plausibly influences you — confirm against the paper before writing:**
* *Veloso 2017* — how backscatter and vegetation change through the season → supports your temporal-signature interpretation.
* *Bargiel 2017* — uses crop-phenology information with radar time series → supports using the crop calendar to design labels/features.
* *Xu 2019* — machine learning on Sentinel-1 time series → supports Random Forest and the effect of time-series length.
* *Mishra 2022* — Indian (Karnal) application → closest regional analogue; compare accuracy and classes.
* *Rege 2022* — combining optical and radar in India → supports using Sentinel-2 as the reference sensor and Sentinel-1 as the classifier input.
A good "adopt/modify/avoid" line: *Adopt* phenology-guided design; *Modify* it by building labels from optical NDVI because there is no field data; *Avoid* reporting a single accuracy figure.

### 7.5 AI-use declaration (edit to match what you actually did)
> Generative AI (Claude, Anthropic) was used to critique my project plan, to draft Google Earth Engine and Python scripts, to draft the webpage template, and to suggest wording. I ran every script myself, inspected the intermediate outputs (NDVI curves, label maps, confusion matrix, charts) and checked them against the crop calendar and the Sentinel-2 imagery. I [changed these thresholds/parameters: …]. I read the five mandatory papers myself and wrote the paper-review matrix from them. I understand the methods and am responsible for the results and their limitations. AI cannot be used to produce field data, and none was fabricated: reference labels were derived from Sentinel-2 as described.

### 7.6 Study-area package (deliverable 3)
Boundary file (GeoJSON or screenshot), location map (screenshot with district context), crop-calendar table, class definitions (Part 0 table), reference-data description (Part 4.6 paragraph). Put them in the repo folder `study_area/`.

---

## PART 8 — If something goes wrong

| Problem | Fix |
|---|---|
| `Asset not found` | `ASSET_ROOT` wrong, or the `ref_labels` task has not finished (Tasks tab, wait for blue tick) |
| `Computation timed out` / `User memory limit exceeded` | In FT2 set `FILTER = 'none'`, `PER_CLASS = 500`, comment out the 5-seed repeat block; run again |
| FT2: `bestKey is null` | no Sentinel-1 scene fully covers the AOI → check AOI geometry/dates, or set `FORCE_PASS`/`FORCE_ORBIT` |
| A class missing from validation | smaller blocks: `BLOCK_DEG = 0.003`, or a different `SEED`; if still missing, merge that class |
| FT1 gives all "Other" | NDVI layers empty → widen `PERIODS` by a week, or raise `CLOUDY_PIXEL_PERCENTAGE` limit |
| Level 2 warning about weak separation | stay with Level 1 |
| Python `rasterio` install fails | use Google Colab (Part 6) |
| Page shows "Data files not found" | open via `python -m http.server` or the GitHub Pages URL, not by double-click; check `docs/data/data.json` exists |
| Map overlay in wrong place | bounds file wrong → re-run `build_site.py` using the **WGS84** GeoTIFF |
| GitHub Pages 404 | Settings → Pages: branch `main`, folder `/docs`; file must be called `index.html`; wait 2 min |

---

## PART 9 — Viva cheat-sheet (10–15 min)
**Your 90-second story:** *"I mapped winter crop groups near Sultanganj, Mainpuri, from Sentinel-1 radar. I had no field survey data, so I made reference labels from Sentinel-2 greenness curves using the crop calendar, and trained a Random Forest on 44 radar features. I tested it on spatial blocks the model never saw and report accuracy as provisional. The map is published on GitHub Pages."*

Likely questions and one-line answers
1. **Why radar?** Winter fog/cloud; radar sees through it and gives regular dates.
2. **VV vs VH?** VH reacts to leaves/stems (volume scattering), VV to the ground surface.
3. **Why average in power, not dB?** dB is logarithmic; averaging must be done on real power.
4. **What is speckle and how did you handle it?** Grainy radar noise; multi-temporal Quegan filter keeps field boundaries.
5. **Why one orbit?** Same viewing angle each date → changes are crops, not geometry.
6. **Where did your training data come from?** Optical NDVI rules from the crop calendar; no field survey; therefore provisional.
7. **Isn't that circular?** Labels come from Sentinel-2 (optical), predictions from Sentinel-1 (radar) — different sensors; but validation is still against the optical reference, so I call it internal.
8. **How did you avoid leakage?** Whole 500 m blocks held out; five repeated splits.
9. **Which classes are confused and why?** Read your own matrix; link to similar timing / small fields / moisture.
10. **What would improve it?** Field GPS data, probability-sample validation, NISAR L-band, more seasons.
11. **Why only crop groups?** Mustard/potato cannot be validated reliably without ground data; sheet §5 allows justified crop-group classification.

---

## APPENDIX — Glossary in plain words
* **Backscatter** — how much of the radar "shout" bounces back.
* **dB (decibel)** — a log scale for echo strength; more negative = weaker echo.
* **NDVI** — greenness number from 0 (bare) to ~0.9 (dense green) from an optical satellite.
* **NDYI** — yellowness number; flowering mustard makes it jump.
* **Speckle** — grainy noise in radar images.
* **Composite / window** — one cleaned image made from all scenes in a 12-day period.
* **Feature** — one number per pixel the model uses (e.g. VH in window 7).
* **Random Forest** — many simple decision trees voting together.
* **Training vs validation** — study material vs the exam the model has never seen.
* **Confusion matrix** — table of "what the map said" vs "what the reference said".
* **OA / PA / UA / F1** — overall correctness; how many real pixels of a class were found; how many pixels labelled a class are right; balance of the last two.
* **Spatial leakage** — cheating by testing on pixels right next to training pixels.
