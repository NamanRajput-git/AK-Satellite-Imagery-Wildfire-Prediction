# Data Sources Guide

This document covers all satellite and ground-based data sources used in 
the Alaska Wildfire Prediction project, including access instructions and 
relevant parameters.

---

## Satellite Data Sources

### 1. Sentinel-2 (ESA) — Primary Optical Source
- **Resolution:** 10m (RGB, NIR), 20m (SWIR)
- **Revisit Frequency:** 5 days
- **Use Case:** Fire risk classification, vegetation health, early warning
- **Key Bands:**
  - B04 (Red), B08 (NIR) → NDVI
  - B08, B12 (SWIR) → NBR (Burn Ratio)
  - B08, B11 (SWIR) → NDWI (Water/Moisture)
  - B02, B03, B04 → True Color Visualization
- **Access Portal:** https://dataspace.copernicus.eu
- **API:** Copernicus Data Space Ecosystem (CDSE)
- **Requires:** Free Copernicus account → add credentials to `.env`

---

### 2. Sentinel-1 (ESA) — SAR Imagery
- **Resolution:** 5m–20m
- **Revisit Frequency:** 6–12 days
- **Use Case:** Vegetation moisture, burned area mapping, cloud-penetrating detection
- **Key Parameters:**
  - VV polarization → surface moisture sensitivity
  - VH polarization → vegetation structure
  - IW (Interferometric Wide Swath) mode recommended for Alaska coverage
- **Access Portal:** https://dataspace.copernicus.eu
- **Note:** SAR data is available regardless of cloud cover or darkness,
  making it especially valuable for Alaska's cloudy seasons.

---

### 3. Landsat 8 & 9 (NASA/USGS)
- **Resolution:** 30m (multispectral), 100m (thermal)
- **Revisit Frequency:** 16 days
- **Use Case:** Pre/post-fire vegetation tracking, burn severity mapping
- **Key Bands:**
  - Band 5 (NIR), Band 4 (Red) → NDVI
  - Band 5, Band 7 (SWIR) → NBR
  - Band 10 (TIRS) → Surface temperature anomalies
- **Access Portal:** https://earthexplorer.usgs.gov
- **API:** NASA EarthData / USGS M2M API
- **Requires:** Free NASA EarthData account

---

### 4. MODIS (Terra/Aqua, NASA)
- **Resolution:** 250m (fire detection), 1km (thermal)
- **Revisit Frequency:** Daily
- **Use Case:** Historical fire perimeters, active fire locations, long-term analysis
- **Key Products:**
  - MOD14/MYD14 → Active Fire detection
  - MOD13 → Vegetation Index time series
  - MCD64A1 → Monthly burned area
- **Access Portal:** https://firms.modaps.eosdis.nasa.gov
- **API:** NASA FIRMS (Fire Information for Resource Management System) — free, no auth required for basic access

---

### 5. VIIRS (Suomi NPP & NOAA-20)
- **Resolution:** 375m (fire detection), 750m (thermal)
- **Revisit Frequency:** Daily
- **Use Case:** Real-time fire monitoring, active hotspot detection
- **Key Products:**
  - VNP14IMGTDL — Near real-time active fire data
  - VJ114IMGTDL — NOAA-20 active fire data
- **Access Portal:** https://firms.modaps.eosdis.nasa.gov/usfs
- **Note:** Preferred over MODIS for real-time detection due to higher 
  spatial resolution at 375m.

---

### 6. ALOS-2 (JAXA)
- **Resolution:** 10m–100m
- **Revisit Frequency:** 14 days
- **Use Case:** L-band SAR for dry fuel detection, terrain change mapping
- **Key Advantage:** L-band penetrates vegetation canopy better than 
  Sentinel-1's C-band, enabling deeper soil moisture estimation
- **Access Portal:** https://www.eorc.jaxa.jp/ALOS/en/dataset/dataset_index.htm
- **Note:** Requires JAXA account; some products need formal data request.

---

## Ground-Based Weather Data

### 1. ERA5 Climate Reanalysis (ECMWF)
- **Coverage:** Global, ~31km resolution
- **Variables Used:** 2m temperature, 10m wind speed (u/v components),
  relative humidity, precipitation, soil moisture
- **Temporal Range:** 1940–present (project uses ~1995–present)
- **Access:** https://cds.climate.copernicus.eu
- **API:** `cdsapi` Python library
- **Requires:** Free CDS account → add API key to `.env`
- **Latency:** ~5 days behind real-time

---

### 2. NOAA NWS Weather Data
- **Coverage:** Alaska weather stations
- **Variables:** Near real-time humidity, wind speed/direction, temperature
- **Access:** https://www.weather.gov/afc (Alaska Forecast Center)
- **API:** NOAA Weather API — https://api.weather.gov (no key required)
- **Use Case:** Supplements ERA5 with near real-time station observations

---

### 3. Alaska Fire Service (AFS) Wildfire Data
- **Coverage:** Alaska statewide
- **Data Includes:** Historical fire perimeters, ignition sources 
  (lightning vs human), fire size, start/end dates
- **Access:** https://fire.ak.blm.gov
- **Format:** Shapefiles, KML, GeoJSON
- **Use Case:** Ground truth labels for model training; ignition source 
  analysis

---

## Data Access Summary Table

| Source | Portal | Auth Required | Format |
|---|---|---|---|
| Sentinel-1 & 2 | Copernicus CDSE | Yes (free) | GeoTIFF |
| Landsat 8 & 9 | USGS EarthExplorer | Yes (free) | GeoTIFF |
| MODIS | NASA FIRMS | No | CSV, Shapefile |
| VIIRS | NASA FIRMS | No | CSV, Shapefile |
| ALOS-2 | JAXA EORC | Yes (free) | GeoTIFF |
| ERA5 | Copernicus CDS | Yes (free) | NetCDF |
| NOAA NWS | api.weather.gov | No | JSON |
| Alaska Fire Service | fire.ak.blm.gov | No | Shapefile, GeoJSON |

---

## Notes on Data Volume

Alaska is a large region (~1.7 million km²). Full-coverage satellite 
downloads can be several GB per acquisition. It is recommended to:

- Define a study area bounding box before downloading (see `alaska_region_guide.md`)
- Use cloud-based processing (Google Earth Engine) for exploratory analysis
- Download locally only for final model training runs