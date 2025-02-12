# Using OpenStreetMap (OSM)

OpenStreetMap (OSM) is used as a basis for analysis because it provides infrastructure data globally. This enables an initial assessment, such as hotspot analysis, in relation to natural hazard risks. Data can be obtained from [Geofabrik](https://download.geofabrik.de/) or extracted using the Overpass API. This notebook demonstrates how we use the [`read_osm_data()`](https://github.com/VU-IVM/DamageScanner/blob/DS1.0/src/damagescanner/osm.py) to retrieve specific assets for analysis.

For further details on tailoring local and regional data, refer to [this page](https://vu-ivm.github.io/GlobalInfraRisk/howto/using_tailor_data.html).

The function supports the following infrastructure systems:

```python
asset_types = [
    "roads",
    "rail",
    "air",
    "telecom",
    "water_supply",
    "waste_solid",
    "waste_water",
    "education",
    "healthcare",
    "power",
    "gas",
    "oil",
    "buildings",
]
```

## Downloading OpenStreetMap (OSM) Data

### Using the `download()` Function

To obtain OpenStreetMap (OSM) data for a specific country, we use the `download.get_country_geofabrik()` function from the [**DamageScanner.download**](https://github.com/VU-IVM/DamageScanner/blob/DS1.0/src/damagescanner/download.py) module (originally developed within the [**osm-flex**](https://osm-flex.readthedocs.io/en/latest/index.html) package). This function retrieves country-specific `.osm.pbf` files from [Geofabrik](https://download.geofabrik.de/), a widely used source for regional OSM extracts.

#### Example Usage:
```python
from damagescanner import download

# Define the country code (ISO 3166-1 alpha-3)
country_iso3 = "NLD"  # Example: Netherlands

# Retrieve the OSM data file path
features = download.get_country_geofabrik(country_iso3)
```

This function requires the **ISO3 country code**, which is used to match available datasets. The dictionary of supported country codes is found in the **DamageScanner config file**:

📌 **[View the country dictionary](https://github.com/VU-IVM/DamageScanner/blob/DS1.0/src/damagescanner/config.py#L46)**

### Handling Countries Not in the Dictionary
If a country is not listed in the dictionary, it means that a predefined extract is unavailable, and a **custom clip** is required from a larger regional or continental file. There are two primary approaches to achieving this:

#### **1. Using GeoPandas with a Bounding Box (Simpler Approach)**
The most recent `geopandas.read_file()` function allows users to clip a dataset to a specific **bounding box (bbox)**:

```python
import geopandas as gpd

# Load a larger OSM dataset (e.g., Europe extract)
osm_data = gpd.read_file("path/to/europe-latest.osm.pbf", bbox=(xmin, ymin, xmax, ymax))
```

📌 **[GeoPandas read_file Documentation](https://geopandas.org/en/stable/docs/reference/api/geopandas.read_file.html)**

#### **2. Using OSM-Flex for Advanced Clipping**
For more precise extraction based on administrative boundaries or complex geometries, the **osm-flex** package provides a tailored clipping approach:

📌 **[OSM-Flex Clipping Documentation](https://osm-flex.readthedocs.io/en/latest/1_clipping_shapes.html)**

This method is more advanced and allows for detailed customization of OSM extracts beyond simple bounding boxes.

## Extracting Data with `read_osm_data()`

The [`read_osm_data()`](https://github.com/VU-IVM/DamageScanner/blob/DS1.0/src/damagescanner/osm.py#L406) function processes `.osm.pbf` files and extracts data related to predefined asset types. It applies OSM query filters stored in [`DICT_CIS_OSM`](https://github.com/VU-IVM/DamageScanner/blob/DS1.0/src/damagescanner/osm.py#L12) to select relevant infrastructure features.

### Example: Retrieving Road Network Data
When extracting data, OSM-based attribute columns (e.g., `highway` for roads) are automatically converted into a standardized `object_type` column. This ensures consistency when linking extracted data to vulnerability curves and maximum damage values.

```python
from damagescanner.osm import read_osm_data

# Define file path and asset type
osm_file = "path/to/osm_data.osm.pbf"
asset_type = "roads"

# Extract road network data
gdf_roads = read_osm_data(osm_file, asset_type)

# Display first few rows
print(gdf_roads.head())
```

### Example: Retrieving Building Footprints

```python
# Extract building footprints
gdf_buildings = read_osm_data(osm_file, "buildings")

# Display first few rows
print(gdf_buildings.head())
```

## Creating the Maximum Damage DataFrame

To link OSM data with damage assessments, maximum reconstruction costs must be defined per infrastructure type. These values must align with the `object_type` column in the extracted OSM dataset. Maximum damage values should be validated using expert input or literature sources.

```python
import pandas as pd

# Define maximum reconstruction costs
max_damage_data = {
    'object_type': ['primary', 'secondary', 'tertiary'],
    'max_reconstruction_cost_per_meter': [1000, 750, 500]  # Example values in currency units per meter
}

# Create DataFrame
max_damage_df = pd.DataFrame(max_damage_data).set_index('object_type')

# Display DataFrame
print(max_damage_df)
```

## Linking Vulnerability Curves

Each infrastructure type requires a vulnerability curve to determine expected damage based on hazard intensity. Vulnerability curves are hazard-specific and should be reviewed for accuracy. A useful reference is [Nirandjan et al. (2024)](https://nhess.copernicus.org/articles/24/4341/2024/nhess-24-4341-2024-discussion.html), but local validation is necessary.

To facilitate vulnerability curve selection, a predefined dictionary linking OSM categories to damage curves is available in [`base_values.py`](https://github.com/VU-IVM/DamageScanner/blob/DS1.0/src/damagescanner/base_values.py). Users can use this as a reference and adjust as needed based on local validation.

```python
import numpy as np

# Define vulnerability curves as a DataFrame
damage_curves_data = {
    'primary': [0.0, 0.2, 0.5, 0.8, 1.0],
    'secondary': [0.0, 0.3, 0.6, 0.9, 1.0],
    'tertiary': [0.0, 0.4, 0.7, 0.9, 1.0]
}

# Create DataFrame
depth_levels = [0, 1, 2, 3, 4]
damage_curves = pd.DataFrame(damage_curves_data, index=depth_levels)
damage_curves.index.rename('Depth', inplace=True)
damage_curves = damage_curves.astype(np.float32)

# Display DataFrame
print(damage_curves)
```

## Visualizing the Data

```python
import matplotlib.pyplot as plt

gdf_roads.plot()
plt.title("Road Networks from OSM")
plt.xlabel("Longitude")
plt.ylabel("Latitude")
plt.show()
```
