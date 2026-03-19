# NYC + NJ Snow Map — Winter 2025–26

Interactive neighborhood-level snowfall visualization covering New York City and nearby New Jersey municipalities.

**Live:** [mekhalraj.github.io/nyc-snow-map](https://mekhalraj.github.io/nyc-snow-map)

**Part 2 of:** [Boston Snow Map](https://mekhalraj.github.io/boston-snow-map) — built after someone on LinkedIn asked for an NYC version.

---

## What It Shows

Seasonal and per-storm snowfall across 262 NYC neighborhoods (NTAs) and ~125 NJ municipalities in Hudson, Bergen, Essex, and Union counties. The dashboard includes:

- Choropleth map with NYC and NJ on the same view
- Storm selector — click any storm to recolor the map
- Borough breakdown — average snowfall per NYC borough
- NJ section — top municipalities per county
- Historical charts — 88 winters at Central Park, plus Newark Airport history
- 3-city comparison — NYC vs NJ vs Boston head-to-head

## Data Sources

| Source | What | Coverage |
|--------|------|----------|
| NOAA GHCN Daily | Daily snowfall | 5 stations: Central Park, LaGuardia, JFK, Newark, Teterboro |
| CoCoRaHS | Volunteer daily snow reports | NY + NJ states, filtered to metro bounding box |
| NYC Open Data | Neighborhood Tabulation Areas (2020) | 262 NTAs across 5 boroughs |
| NJOGIS | Municipal boundaries | Hudson, Bergen, Essex, Union counties |

## Pipeline

Google Colab notebook: `nyc_snow_pipeline.ipynb`

The pipeline fetches data from all sources, runs IDW (inverse distance weighting) spatial interpolation from station locations to neighborhood/municipality centroids, and exports GeoJSON + JSON files for the frontend.

**Key difference from Boston:** Boston used 1 NOAA station and street-level segments. NYC uses 5 NOAA stations, CoCoRaHS from two states, and neighborhood-level polygons instead of streets.

### Running the Pipeline

1. Open `nyc_snow_pipeline.ipynb` in Google Colab
2. Paste your NOAA API key in Cell 3 (get one free at [ncdc.noaa.gov](https://www.ncdc.noaa.gov/cdo-web/token))
3. Run all cells
4. Download the `outputs/` folder

### Pipeline Outputs

| File | Description | Size |
|------|-------------|------|
| `nyc_neighborhoods.geojson` | 262 NTAs with per-storm snowfall | ~5 MB |
| `nyc_boroughs.geojson` | 5 boroughs with aggregated stats | ~4 MB |
| `nj_municipalities.geojson` | NJ towns with per-storm snowfall | varies |
| `nyc_storms.json` | Storm events with station data | ~2 KB |
| `nyc_historical.json` | Central Park seasonal totals 1938–2026 | ~19 KB |
| `nj_historical.json` | Newark Airport seasonal totals | ~19 KB |

## Frontend

Single `index.html` file. No build step, no framework.

- MapLibre GL JS with CARTO Positron tiles (free, no API token)
- Vanilla JS for all rendering
- Canvas-based historical charts
- Canvas snowfall animation
- Playfair Display + DM Sans typography

### Running Locally

```
# clone the repo
git clone https://github.com/mekhalraj/nyc-snow-map.git
cd nyc-snow-map

# serve locally (any static server works)
python3 -m http.server 8000
# open http://localhost:8000
```

## Repo Structure

```
nyc-snow-map/
├── index.html                    # Dashboard (single file)
├── nyc_neighborhoods.geojson     # NYC NTAs with snowfall
├── nyc_boroughs.geojson          # Borough boundaries + stats
├── nj_municipalities.geojson     # NJ towns with snowfall
├── nyc_storms.json               # Storm events
├── nyc_historical.json           # Central Park history
├── nj_historical.json            # Newark Airport history
├── nyc_snow_pipeline.ipynb       # Data pipeline (Colab)
└── README.md
```

## Tech Stack

| Layer | Tool |
|-------|------|
| Data pipeline | Python, pandas, geopandas, scipy (IDW), CoCoRaHS-Download-Tool |
| Weather data | NOAA GHCN API, CoCoRaHS |
| Boundaries | NYC Open Data, NJOGIS |
| Map | MapLibre GL JS, CARTO Positron |
| Charts | Canvas 2D |
| Fonts | Playfair Display, DM Sans, DM Mono |
| Hosting | GitHub Pages |

## Season Summary (2025–26)

| Metric | NYC (Central Park) | NJ (Newark) | Boston (Logan) |
|--------|-------------------|-------------|----------------|
| Season total | 43.4" | 54.5" | 62.5" |
| Historical avg | 26.6" | 28.7" | 42.5" |
| All-time rank | #15 | #10 | #10 |
| Storms | 5 | 5 | 7 |
| Areas mapped | 262 | ~125 | 41,000+ |

## Related

- [Boston Snow Map](https://mekhalraj.github.io/boston-snow-map) — street-level snowfall for Greater Boston
- [LinkedIn post](https://www.linkedin.com/in/mekhalrajr/) — project writeup

---

Built by [Mekhal Raj](https://www.linkedin.com/in/mekhalrajr/)
