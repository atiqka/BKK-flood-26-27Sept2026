### BANGKOK FLOOD MAP (BMA) - 26-27 September 2026 ###

Author: Atiq Kainan Ahmed. Email: atiqka@yahoo.com


This Python script produces a district-level flood severity map of Bangkok for the 26-27 September 2026 flood event. It combines a high-resolution satellite flood proxy map with live river gauge data, and exports a print-ready PNG with English labels.


## What it does

- Downloads Bangkok district boundaries (Thailand districts GeoJSON) and keeps only Bangkok (province code 10).
- Downloads the satellite flood map (EOS-RS Flood Proxy Map, Sentinel-1 based) from the Humanitarian Data Exchange (HDX) and extracts the GeoTIFF.
- Calculates flood severity per district: the percentage of valid raster pixels flagged as flooded inside each district polygon.
- Fetches gauge stations from the ThaiWater API, retrieves the water level for the target date, and compares it with the bank level.
- Plots a map with districts filled by severity, gauge stations on top, district name labels, and two legends. Saved as a 300 dpi PNG.

## Output
The map output can be found here https://github.com/atiqka/BKK-flood-26-27Sept2026/blob/main/BKK_flood26-27Sept2026.png

Bangkok_Districts_Filled_by_Flood_Severity.png


## District severity classes (share of district area flooded):

Class	Flooded area
Very High	≥ 40%
High	20–40%
Moderate	8–20%
Low	2–8%
Very Low / None	< 2%

## Gauge station status (water level minus bank level):

Status	Condition
Over-bank	≥ +0.3 m
Near bank	between −0.3 m and +0.3 m
Normal	below −0.3 m
No data	level or bank reference missing

## Requirements

Python 3.9+
Packages: requests, numpy, pandas, geopandas, shapely, matplotlib, rasterio

Install with pip:

bash
pip install requests numpy pandas geopandas shapely matplotlib rasterio

geopandas and rasterio depend on GDAL. If pip fails, install them with conda instead:

bash
conda install -c conda-forge geopandas rasterio matplotlib requests
Usage
bash
python bangkok_flood_map.py

Rename the command to match your script's file name. On first run the script downloads the districts file and the flood map ZIP into the working directory. Later runs reuse them.

If the HDX download returns an HTML page instead of a ZIP, download the file manually from the HDX dataset page and save it as EOSRS_Flood_Proxy_Map_Sep2026.zip next to the script, then run it again.

## Configuration

Edit the settings at the top of the script:

Setting	Default	Purpose
ZIP_PATH	EOSRS_Flood_Proxy_Map_Sep2026.zip	Local path of the flood map ZIP
EXTRACT_DIR	eosrs_flood_sep2026	Folder the ZIP is extracted to
DISTRICTS	thailand_districts.geojson	Local districts file
OUTPUT_PNG	Bangkok_Districts_Filled_by_Flood_Severity.png	Output image
TARGET_DATE	2026-09-26	Date used for gauge water levels
MAX_WORKERS	10	Parallel requests to the ThaiWater API

One can also adjust the severity thresholds in severity_class(), the flooded-pixel test (valid > 0), and the station bounding box (gdf_st.cx[100.3:100.95, 13.5:14.0]).

## Data sources
Flood map: EOS-RS Flood Proxy Map, Bangkok, via the Humanitarian Data Exchange
District boundaries: OpenGISData-Thailand (districts.geojson)
Gauge stations and water levels: ThaiWater API (Hydro-Informatics Institute, Thailand)

Please credit the original data providers when you reuse the map, and check each source for its own license and terms of use.

## Limitations
Flood severity is a proxy from satellite imagery. It shows the share of each district detected as water, not measured flood depth, and it can miss flooding under dense urban cover or vegetation.
The flooded-pixel rule (value > 0) assumes the raster marks water with positive values. Check the raster's documentation and adjust if needed.
The ThaiWater API is a live service. Its endpoints, fields, or availability may change, and the script uses the last reading found for TARGET_DATE.
The satellite product is dated 29 September 2026 in its file name, while the gauge date is 26 September. Keep this in mind when interpreting the combined map.

## Workflow structure
.

├── bangkok_flood_map.py

├── README.md

└── (generated on first run)

    ├── thailand_districts.geojson
    
    ├── EOSRS_Flood_Proxy_Map_Sep2026.zip
    
    ├── eosrs_flood_sep2026/
    
    └── Bangkok_Districts_Filled_by_Flood_Severity.png
    
## License

MIT License

Copyright (c) 2026 Atiq Kainan Ahmed.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
