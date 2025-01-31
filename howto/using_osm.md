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

## Extracting Data with `read_osm_data()`

The `read_osm_data` function processes `.osm.pbf` files and extracts data related to predefined asset types. It uses a set of OSM keys and query filters to select relevant infrastructure features.

### Function Overview

```python
def read_osm_data(osm_path, asset_type):
    """
    Extracts asset data from an OSM file.
    
    Parameters:
    - osm_path (str or Path): Path to the `.osm.pbf` file.
    - asset_type (str): Type of infrastructure to extract (e.g., 'roads', 'buildings').
    
    Returns:
    - GeoDataFrame with extracted asset data.
    """
```

### Example: Retrieving Road Network Data

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

## Filtering and Cleaning Data

The function applies predefined OSM queries stored in `DICT_CIS_OSM`. Each infrastructure type is mapped to specific OSM tags.

Example structure:

```python
DICT_CIS_OSM = {
    "roads": {
        "osm_keys": ["highway", "name", "maxspeed"],
        "osm_query": {"highway": ["motorway", "primary", "secondary"]},
    },
     "telecom": {
        "osm_keys": ["man_made", "tower_type", "name"],
        "osm_query": {
            "man_made": ["mast", "communications_tower"],
            "tower_type": ["communication"],
        }
    },    
}
```

This filtering ensures that only relevant features are included in the output.

## Visualizing the Data

```python
import matplotlib.pyplot as plt

gdf_roads.plot()
plt.title("Road Networks from OSM")
plt.xlabel("Longitude")
plt.ylabel("Latitude")
plt.show()
```

This function processes and structures OSM data efficiently, enabling its integration into risk assessments. More details on the implementation are available in the [DamageScanner OSM module](https://github.com/VU-IVM/DamageScanner/blob/DS1.0/src/damagescanner/osm.py).
