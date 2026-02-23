# Alaska Region Reference Guide

This document provides Alaska-specific geospatial reference information 
for consistent data processing across the project.

---

## Coordinate Reference System

**Standard CRS for this project: EPSG:3338 (Alaska Albers Equal Area)**

All processed outputs should be reprojected to EPSG:3338 before analysis.
```python
import rasterio
from rasterio.warp import reproject, Reproject, calculate_default_transform

# Reproject any raster to Alaska Albers
dst_crs = "EPSG:3338"
```

**Why Alaska Albers?**
- Equal-area projection — preserves relative area for accurate fire 
  extent measurement
- Optimized for Alaska's geographic extent and high latitude
- Standard used by Alaska state agencies including Alaska Fire Service

---

## Recommended Bounding Boxes

All coordinates in WGS84 (EPSG:4326):
```python
# Full Alaska extent
ALASKA_BOUNDS = {
    "west": -179.5,
    "east": -129.0,
    "south": 54.5,
    "north": 71.5
}

# Interior Alaska (highest wildfire activity)
INTERIOR_ALASKA_BOUNDS = {
    "west": -155.0,
    "east": -141.0,
    "south": 63.0,
    "north": 68.5
}

# Fairbanks region (recommended for initial development & testing)
FAIRBANKS_BOUNDS = {
    "west": -149.0,
    "east": -146.0,
    "south": 64.5,
    "north": 65.5
}
```

**Recommendation:** Use the Fairbanks bounding box for local development 
and testing. It is historically one of the most fire-prone regions in 
Alaska and keeps data volumes manageable.

---

## Fire-Prone Regions in Alaska

| Region | Notes |
|---|---|
| Interior Alaska (Fairbanks, Tanana Valley) | Highest fire frequency; boreal forest + dry summers |
| Yukon Flats | Large lightning-caused fires; remote wilderness |
| Kenai Peninsula | Mixed forest; human-caused ignitions more common |
| Seward Peninsula | Tundra fires; increasing frequency due to warming |
| Alaska Range Foothills | Transitional zone; fire behavior varies significantly |

---

## Alaska Fire Season

| Period | Conditions |
|---|---|
| May–June | Fire season onset; dry winds, low humidity |
| July | Peak fire activity; lightning storms common in interior |
| August | Season tapering; some late-season fires possible |
| September–April | Low/no fire activity |

Satellite imagery from **May through August** is most relevant for 
pre-fire risk analysis. ERA5 weather data should cover at minimum the 
**30-day window prior** to any fire event used as a training label.

---

## Common Geospatial Issues in Alaska

**Antimeridian crossing:** Western Alaska (Aleutian Islands) crosses the 
180° meridian. If your bounding box spans this, use Alaska Albers 
(EPSG:3338) from the start to avoid geometry errors.

**High latitude distortion:** Web Mercator (EPSG:3857) severely distorts 
Alaska. Never use it for area calculations or model inputs.

**Cloud cover:** Alaska has persistent cloud cover, especially coastal 
regions. Always apply cloud masking to Sentinel-2 imagery using the SCL 
band before any analysis.