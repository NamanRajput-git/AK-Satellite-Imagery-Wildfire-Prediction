# Alaska Wildfire Prediction Using Satellite Imagery

A hybrid deep learning project to predict wildfire risk in Alaska by 
integrating optical, thermal, and SAR satellite imagery with ground-based 
weather data.

> **Status:** Research Stage | GSoC 2026 Project  
> **Mentors:** Yali Wang & Arghya Kusum Das — University of Alaska

---

## Overview

Alaska experiences large-scale wildfires annually across boreal forests, 
tundra, and remote wilderness. Traditional prediction methods rely on 
weather data and human observations which can be delayed or inaccurate in 
remote areas. This project leverages real-time satellite imagery to 
provide high-resolution insights into vegetation health, thermal anomalies, 
soil moisture, and fuel dryness — enabling earlier and more accurate 
wildfire risk prediction.

The core output is a model that classifies regions as **High Fire Risk**, 
**Moderate Risk**, or **No Risk** within a defined time window (1, 3, or 
6 months).

---

## Data Sources

This project integrates multi-sensor satellite imagery and ground-based 
weather data.

| Source | Type | Resolution | Revisit | Use Case |
|---|---|---|---|---|
| Sentinel-2 (ESA) | Optical | 10m–20m | 5 days | Fire risk classification, NDVI/NBR |
| Sentinel-1 (ESA) | SAR | 5m–20m | 6–12 days | Vegetation moisture, burned area |
| Landsat 8 & 9 (NASA/USGS) | Optical/Thermal | 30m–100m | 16 days | Burn severity mapping |
| MODIS (NASA) | Thermal | 250m–1km | Daily | Historical fire perimeters |
| VIIRS (NOAA) | Thermal | 375m–750m | Daily | Real-time hotspot detection |
| ALOS-2 (JAXA) | SAR (L-band) | 10m–100m | 14 days | Dry fuel, terrain mapping |
| ERA5 (ECMWF) | Weather Reanalysis | ~31km | Hourly | Temperature, wind, humidity |
| NOAA NWS | Weather Stations | Point data | Real-time | Near real-time weather |
| Alaska Fire Service | Fire Records | Vector | — | Historical ignition labels |

For detailed access instructions, API references, and download guidance 
see [docs/data_sources.md](docs/data_sources.md).

---

## Geospatial Reference

- **Standard CRS:** EPSG:3338 (Alaska Albers Equal Area)
- **Primary Study Area:** Interior Alaska (Fairbanks region for development)
- **Fire Season:** May–August

For bounding boxes, fire-prone regions, and Alaska-specific geospatial 
notes see [docs/alaska_region_guide.md](docs/alaska_region_guide.md).

---

## Expected Outcomes

**Minimum Viable Product (MVP):**
- Fire risk classification model (High / Moderate / No Risk)
- Satellite + weather data preprocessing pipeline
- Web-based GIS dashboard for visualizing fire-prone regions
- Model performance report with fire risk metrics

**Pipeline Stages:**
1. Sentinel-2 preprocessing — band selection, cloud removal, geospatial cropping
2. Sentinel-1 SAR analysis — vegetation density and soil moisture extraction
3. Weather data integration — 30 years of ERA5 time-series for Alaska
4. Hybrid model training — CNN-LSTM or equivalent spatial-temporal architecture

---

## Quick Start

### Prerequisites
- Python 3.8+
- conda or venv (recommended)

### Setup
```bash
git clone https://github.com/YaliWang2019/AK-Satellite-Imagery-Wildfire-Prediction.git
cd AK-Satellite-Imagery-Wildfire-Prediction

# Create environment
conda create -n ak-wildfire python=3.10
conda activate ak-wildfire

# Install dependencies
pip install -r requirements.txt

# Configure API credentials
cp .env.example .env
# Edit .env with your Copernicus and ECMWF API keys
```

For full setup instructions including GDAL installation and 
troubleshooting see [docs/setup.md](docs/setup.md).

---

## Required Skills

- Python (required)
- Deep learning and machine learning experience (required)
- Multi-band satellite imagery processing (preferred)
- Geospatial data processing — ArcGIS Pro, GDAL, rasterio (preferred)
- Remote sensing fundamentals (preferred)

---

## Project Info

| Field | Details |
|---|---|
| Effort | 350 Hours |
| Difficulty | Medium / Hard |
| Program | GSoC 2026 |
| Discussion Forum | [GitHub Discussions](https://github.com/YaliWang2019/AK-Satellite-Imagery-Wildfire-Prediction/discussions) |

---

## Contributing

We welcome contributions! Please read 
[CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

This project is currently in the research and setup stage. Good first 
contributions include documentation improvements, data pipeline 
prototypes, and exploratory notebooks.