# Introducing `DamageScanner()`

`DamageScanner()` is a Python toolkit designed for **direct damage assessments of natural hazards**. Initially developed for **flood damage assessments**, it can be used for any hazard that requires a **vulnerability curve** (i.e., a one-dimensional relation between hazard intensity and damage). While the function is straightforward, **careful preparation of inputs is crucial**. 

The core concept behind `DamageScanner()` is **spatial intersection and damage estimation**:
1. **Exposure Analysis** - Determines which assets overlap with a hazard layer.
2. **Damage Calculation** - Applies vulnerability curves to estimate damage.
3. **Risk Assessment** - Aggregates damage across different hazard return periods.

The function is designed to work efficiently with **raster-based hazard data** and **vector-based exposure data**. However, here we will focus on **vector-based** analysis, given that most infrastructure data is available as object-based information.


## The Core `DamageScanner` Class Explained

At its core, `DamageScanner()` requires **four essential inputs**:

### **1. Hazard Data**
Hazard data represents the intensity of a natural hazard, such as flood depth, wind speed, or ground shaking. This data is typically stored in:
- **Raster format** (`.tif`, `.tiff`, `.nc`)
- **Path objects** pointing to hazard datasets

If provided as a string, it is automatically converted into a `Path` object.

### **2. Exposure Data (Referred to as Feature Data)**
Exposure data, referred to as **feature data** in the `DamageScanner` class to avoid confusion with the `exposure()` function, contains infrastructure assets at risk, such as roads, buildings, or power plants. It can be provided as:
- **Vector format** (`.shp`, `.gpkg`, `.pbf`, `.geoparquet`, `.geofeather`)
- **Raster format** (`.tif`, `.tiff`, `.nc`) for grid-based exposure data
- **GeoDataFrames (`geopandas.GeoDataFrame`)**
- **Pandas DataFrames (`pandas.DataFrame`)**

If a string path is provided, `DamageScanner` checks the file extension to determine if it's vector or raster-based feature data.

### **3. Vulnerability Curves**
Vulnerability curves describe the relationship between hazard intensity and expected damage. These can be provided as:
- **Pandas DataFrame (`pandas.DataFrame`)**
- **CSV file (`.csv`)**
- **Path object pointing to a CSV file**

If provided as a string, it is converted into a `Path` object.

### **4. Maximum Damage Values**
Maximum damage (`maxdam`) represents the total potential loss for each asset type. It can be provided as:
- **Pandas DataFrame (`pandas.DataFrame`)**
- **CSV file (`.csv`)**
- **Path object pointing to a CSV file**

As with vulnerability curves, `maxdam` is converted into a `Path` object if provided as a string.

Before running `DamageScanner()`, users should ensure that **data formats are correct and consistent**. More details on data preparation can be found in:
- [Using Local Data](https://vu-ivm.github.io/GlobalInfraRisk/howto/using_tailor_data.html)
- [Using OpenStreetMap](https://vu-ivm.github.io/GlobalInfraRisk/howto/using_osm.html)
- [Vulnerability Curves](https://vu-ivm.github.io/GlobalInfraRisk/intro/vulnerability.html)
- [Data Preparation](https://vu-ivm.github.io/GlobalInfraRisk/howto/data_preparation.html)
- [Running `DamageScanner()` Locally](https://vu-ivm.github.io/GlobalInfraRisk/howto/run_locally.html)


## Running the `DamageScanner` Functions

To use `DamageScanner()`, the four required inputs **must be specified first** before running any of its core functions.

```python
from damagescanner import DamageScanner
import pandas as pd

# Define the required inputs
hazard = "path/to/hazard_data.tif"
feature_data = "path/to/exposure_data.shp"
curves = "path/to/vulnerability_curves.csv"
maxdam = "path/to/maxdam.csv"

# Initialize DamageScanner
scanner = DamageScanner(hazard, feature_data, curves, maxdam)
```

### **1. Exposure Analysis (`exposure()`)**
This function identifies which assets intersect with the hazard area.

```python
# When running exposure analysis alone, curves and maxdam should be empty DataFrames
curves = pd.DataFrame()
maxdam = pd.DataFrame()

scanner = DamageScanner(hazard, feature_data, curves, maxdam)
exposed_assets = scanner.exposure()
print(exposed_assets.head())
```
- **For raster-based feature data**: Reads `GeoTIFF` or `NetCDF` exposure layers.
- **For vector-based feature data**: Reads shapefiles (`.shp`), geopackages (`.gpkg`), and OpenStreetMap (`.pbf`).
- The `curves` and `maxdam` arguments **must be provided as `pd.DataFrame()` placeholders** when running `exposure()` alone.

### **2. Damage Calculation (`calculate()`)**
Once initialized with the required inputs, `calculate()` applies the vulnerability curves to estimate damage.

```python
damage_results = scanner.calculate()
print(damage_results.head())
```
- Uses vulnerability curves stored as `.csv` or a **pandas DataFrame**.
- Uses maximum damage values stored in the same format.
- Supports **multiple vulnerability curves** for different asset types.
- Outputs estimated direct damages per asset.

### **3. Risk Assessment (`risk()`)**
For long-term risk evaluation across multiple hazard return periods:

```python
hazard_dict = {
    10: "hazard_10_year.tif",
    50: "hazard_50_year.tif",
    100: "hazard_100_year.tif"
}

risk_results = scanner.risk(hazard_dict)
print(risk_results.head())
```
- Computes expected annual damages by integrating multiple hazard scenarios.
- Supports **multiple vulnerability curves** for different asset types.