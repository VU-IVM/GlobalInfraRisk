# Introducing `DamageScanner()`

`DamageScanner()` is a Python toolkit designed for **direct damage assessments of natural hazards**. Initially developed for **flood damage assessments**, it can be used for any hazard that requires a **vulnerability curve** (i.e., a one-dimensional relation between hazard intensity and damage). While the function is straightforward, **careful preparation of inputs is crucial**. 


## How `DamageScanner()` Works

The core concept behind `DamageScanner()` is **spatial intersection and damage estimation**:
1. **Exposure Analysis** - Determines which assets overlap with a hazard layer.
2. **Damage Calculation** - Applies vulnerability curves to estimate damage.
3. **Risk Assessment** - Aggregates damage across different hazard return periods.

The function is designed to work efficiently with **raster-based hazard data** and **vector-based exposure data**. However, here we will focus on **vector-based** analysis, given that most infrastructure data is available as object-based information.

## Required Inputs

### **1. Hazard Data (Raster Format)**
The hazard dataset represents hazard intensity, such as **flood depth maps** or **earthquake shaking intensity**. This data is usually stored in **GeoTIFF (.tif)** format.

```python
import xarray as xr

# Load a raster hazard dataset
hazard = xr.open_dataset("path/to/hazard_data.tif", engine="rasterio")
```

### **2. Exposure Data (Vector Format)**
Exposure data includes infrastructure assets that may be affected by the hazard. This is stored in a **shapefile (.shp), GeoJSON (.geojson), or Geopackage (.gpkg)**.

```python
import geopandas as gpd

# Load exposure dataset
gdf_exposure = gpd.read_file("path/to/exposure_data.shp")
```
The dataset should include an `object_type` column that categorizes asset types (e.g., roads, power plants, buildings).

### **3. Vulnerability Curves**
Vulnerability curves translate hazard intensity (e.g., flood depth) into expected damage levels.

```python
import pandas as pd

# Load vulnerability curves
curves = pd.read_csv("path/to/vulnerability_curves.csv")
```
Each asset type should have an assigned vulnerability curve.

### **4. Maximum Damage Values**
Maximum damage (`maxdam`) represents the total reconstruction cost per asset type.

```python
maxdam = pd.read_csv("path/to/maxdam.csv")
```

## Running `DamageScanner()`

Once the inputs are prepared, we can run an exposure analysis and estimate damages.

### **Step 1: Initialize `DamageScanner`**
```python
from damagescanner import DamageScanner

scanner = DamageScanner(hazard, gdf_exposure, curves, maxdam)
```

### **Step 2: Extract Exposure Data**
```python
exposed_assets = scanner.exposure()
print(exposed_assets.head())
```
This function identifies which infrastructure assets are exposed to the hazard.

### **Step 3: Calculate Direct Damages**
```python
damage_results = scanner.calculate()
print(damage_results.head())
```
This function applies vulnerability curves to estimate direct damages for each exposed asset.

### **Step 4: Perform a Risk Assessment**
For multiple hazard events, such as **flood return periods**, use:

```python
hazard_dict = {
    10: "hazard_10_year.tif",
    50: "hazard_50_year.tif",
    100: "hazard_100_year.tif"
}

risk_results = scanner.risk(hazard_dict)
print(risk_results.head())
```
This function integrates damages across different hazard return periods to assess long-term risk.

## Key Considerations
- **Ensure coordinate system consistency** - Both hazard and exposure data should use **EPSG:4326** unless transformed.
- **Check exposure geometries** - Convert points/lines to polygons if needed.
- **Use validated vulnerability and max damage values** for accurate results.

For more details on input preparation, refer to:
- [Using Local Data](https://vu-ivm.github.io/GlobalInfraRisk/howto/using_local_asset_data.html)
- [Using OpenStreetMap](https://vu-ivm.github.io/GlobalInfraRisk/howto/using_openstreetmap.html)
- [Vulnerability Curves](https://vu-ivm.github.io/GlobalInfraRisk/intro/vulnerability.html)
- [Data Preparation](https://vu-ivm.github.io/GlobalInfraRisk/howto/data_preparation.html)

With well-prepared data, `DamageScanner()` provides a flexible and powerful framework for impact and risk assessments.
