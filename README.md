# Echofield

Radar from space, watching desert farms change.

Echofield uses NASA-ISRO NISAR L-band radar to follow center-pivot farms in the Basaita area of Al Jouf, Saudi Arabia, from June to October 2026, and checks the radar against NASA's optical HLS imagery.

NASA Space Apps Challenge 2026, challenge: Dancing with the SARs. Team Waselah (Waselah AlAmoudi).

**Live site:** https://alamoudyyw-afk.github.io/echofield/

## What is in this repository

| File | What it is |
|---|---|
| `echofield.ipynb` | The full analysis, with outputs, as run in Google Colab |
| `index.html` | The website: one page of HTML, CSS and JavaScript, no build step |
| `hv_*.png`, `echofield_season.gif` | NISAR HV images for each date |
| `change.png`, `timeline_corrected.png`, `two_channels.png`, `active_area.png`, `fields_detected.png` | Figures from the radar analysis |
| `ndvi_validation.png`, `ndvi_scatter.png` | Radar compared with optical NDVI |
| `explorer_bg.png` | Background image for the field explorer on the site |
| `context.png` | Google Earth photo of the area, for context only, not used in the analysis |

## Data

- **Radar:** NISAR L2 GCOV provisional products (frequency A, HH and HV, 20 m grid, UTM zone 37N), track 71, frame 17, ascending. Nine dates from June 19 to October 17, 2026. September 23 has no HV channel and is skipped. Accessed from the Alaska Satellite Facility DAAC through NASA Earthdata.
- **Optical:** NASA Harmonized Landsat and Sentinel-2 (HLS, HLSS30 and HLSL30), accessed from the LP DAAC through NASA Earthdata. For each radar date, the clearest scene within 6 days is used; all nine matched within 0 to 1 day.
- Study window: 20 km by 20 km centred on 30.14° N, 38.46° E.

## How to reproduce

1. Create a free NASA Earthdata account at https://urs.earthdata.nasa.gov.
2. Open `echofield.ipynb` in Google Colab (File, Upload notebook).
3. Run the cells in order. The first cell installs the packages and asks for your Earthdata login.
4. Only the 20 km window is read from each file in the cloud (about 20 seconds per date), so nothing large is downloaded.
5. In the field-checking cell, the list `bad` holds the candidate circles rejected by visual inspection. Detection order can change if the code or data change, so check the numbered map before reusing the list.
6. The last cells save every figure, plus the data for the website's field explorer.

## Method in brief

1. Speckle reduced with a 5 by 5 average; values converted to decibels.
2. Noise measured over empty desert (0.50 dB); change counted only above 1.5 dB (three times the noise).
3. Change regions: connected groups of changed pixels larger than 12 ha. These are regions, not verified field boundaries.
4. Every date measured relative to the same desert patch, to remove offsets between dates.
5. Active area: HV more than 4 dB above desert, tested at 3 and 5 dB; buildings masked where median HH is more than 12 dB above desert.
6. Field detection: Hough circle transform on the season maximum, 55 candidates, 38 confirmed by visual inspection.
7. Validation: NDVI from HLS sampled at the same pixels; Spearman correlation pooled and per field, plus a confusion matrix at HV 4 dB and NDVI 0.3.

## Key results

- 33 regions where HV increased and 19 where it decreased between June 19 and October 17.
- Active vegetated area rose from 19.4 km² to 29.1 km² (4 dB threshold); the rise holds at 3 and 5 dB.
- Active confirmed fields: 4 in June, peak of 17 on October 5.
- Radar against optical NDVI: pooled Spearman ρ = 0.50 over 342 field-date pairs; per field, 37 of 38 positive, median ρ = 0.72; 58% agreement on active or inactive, with almost all disagreement being green fields below the radar threshold.

## Limitations

Radar cannot name the crop. Both channels respond to moisture, roughness and vegetation together, so readings such as "irrigation before growth" are hypotheses. NISAR revisits every 12 days and the August 6 pass is missing. The products are provisional. L-band sees little of short or young crops. The window covers one 20 km sample in June to October, outside the main winter wheat season.

## Use of AI

Claude helped with research, the Python analysis code, the website code and drafting text. ChatGPT helped review the code and lay out the slides. All analysis was run, checked and interpreted by the author, who made the visual field checks and the design decisions.

## License

MIT, see `LICENSE`.
