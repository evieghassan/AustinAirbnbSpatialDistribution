# Airbnb Spatial Analysis — Austin, Texas

Computational essay for **ENVS252/363/563 Geographic Data Science** (University of Liverpool).  
Analyses Airbnb listings in Austin, TX against Census demographics to explore spatial inequality and gentrification pressures.

## Research Question

*How do short-term rental (Airbnb) distributions relate to income, ethnicity, and housing pressures in Austin?*

## Key Findings

- **Listings concentrate in the urban core** — highest density in central ZIP codes, following a classic monocentric city pattern.
- **Price gradient** — central areas show mid-range prices (high supply/competition); NW suburbs show higher prices (scarcer, larger properties).
- **Income–ethnicity divide** — West Austin: high income, low Hispanic %. East Austin: lower income, higher Hispanic %.
- **Gentrification signal** — lower-priced Airbnbs cluster in East Austin's lower-income tracts, consistent with displacement pressure on vulnerable residents.
- **Spatial autocorrelation** — Local Moran's I confirms statistically significant High–High (West) and Low–Low (East) income clusters.
- **Geodemographic clusters** — k-means (k=4) on income, Hispanic %, density reveals four distinct neighbourhood types; Clusters 3 & 4 (East) show highest need for policy intervention.

## Methodology

| Step | Tools / Data |
|------|--------------|
| **CRS** | WGS84 (EPSG:4326) — global standard, compatible with Inside Airbnb coordinates |
| **Airbnb data** | Inside Airbnb `listings.csv` — lat/lon, nightly price, availability |
| **Demographics** | US Census ACS 2022 (via `tidycensus`): median household income (B19013_001), Hispanic population (B03003), tract geometry |
| **Areal units** | Census tracts (primary), ZCTAs (ZIP Code Tabulation Areas) for listing aggregation |
| **Spatial join** | `st_join` (listings → tracts/ZCTAs) |
| **Price cleaning** | `parse_number()` on price strings → `price_num` |
| **Mapping** | `tmap` — quantile breaks, viridis (colour-blind safe), custom breaks for price |
| **Spatial stats** | `spdep` — Queen contiguity weights, Local Moran's I (LISA) on income |
| **Clustering** | k-means (k=4) on scaled income, % Hispanic, population density |
| **Visualisation** | Side-by-side maps (listings vs price, income vs ethnicity, LISA, clusters) |

## Reproducibility

### Requirements

- R ≥ 4.2
- Quarto ≥ 1.4
- R packages:
  ```r
  install.packages(c("tidyverse", "tidycensus", "sf", "tmap", "spdep", "tigris", "plyr", "readr"))
  ```
- **Census API key** — get one at <https://api.census.gov/data/key_signup.html> and set in script:
  ```r
  census_api_key("YOUR_KEY_HERE")
  ```

### Data

Place `listings.csv` (from [Inside Airbnb Austin](https://insideairbnb.com/get-the-data/)) in the project root.

> The repository does **not** include the raw listings CSV (too large / licensing). Download it yourself and save as `listings.csv`.

### Render

```bash
quarto render ENVS2526-363-563.2.mine.qmd
```

Outputs a self-contained HTML (`ENVS2526-363-563.2.mine.html`) with all maps, code, and narrative.

## Project Structure

```
GDS2/
├── ENVS2526-363-563.2.mine.qmd   # Main Quarto document
├── listings.csv                   # (you add this — not tracked)
├── README.md                      # This file
└── _freeze/                       # Auto-generated on render (cached outputs)
```

## Figures Overview

| Figure | Description |
|--------|-------------|
| **Map 1.1** | Airbnb listings per ZCTA (quantile, viridis) |
| **Map 1.2** | Average nightly price per ZCTA (custom breaks, viridis) |
| **Map 2.1** | Median household income by tract (quantile, viridis) |
| **Map 2.2** | % Hispanic population by tract (quantile, viridis) |
| **Map 3** | Log(Airbnb price) points overlaid on income polygons (plasma vs viridis) |
| **Map 4** | Local Moran's I (LISA) clusters for median income — High-High, Low-Low, High-Low, Low-High |
| **Map 5** | k=4 geodemographic clusters (income, % Hispanic, density) |

## References

Full bibliography in the Quarto document. Key sources:

- Gold (2019) *Community consequences of Airbnb* — Washington Law Review
- Smith (2018) *Renting diversity: Airbnb as modern housing discrimination* — DePaul Law Review
- Bloch & Quarmby (2024) *Environmental racism, segregation and discrimination: Gypsy and Traveller sites in GB* — Critical Social Policy
- Greenfields & Brindley (2016) *Impact of insecure accommodation on Gypsies' and Travellers' health* — The Traveller Movement
- ACS 2022 5-year estimates via Census API

## License

Academic coursework. Code and analysis © 2024 Evie Hassan (student ID 201730962).  
Data sources: Inside Airbnb (CC-BY-SA 4.0), US Census Bureau (public domain).

## Contact

Evie Hassan — BA Geography, University of Liverpool  
[LinkedIn](https://www.linkedin.com/in/evie-hassan-4bab432a1/)