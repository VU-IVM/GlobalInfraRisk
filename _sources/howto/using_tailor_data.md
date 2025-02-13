# Using Local Data

OpenStreetMap provides global infrastructure data, but local datasets can provide more detail for risk assessments. To integrate local data with the `damagescanner` tool, the data must follow a specific structure. This guide explains how to format infrastructure datasets, link them to maximum damage estimates, and incorporate vulnerability curves for natural hazard assessments.

## Data Requirements

For local data to be compatible with `damagescanner`, it must:
1. **Contain an `object_type` column**: This column categorizes assets (e.g., roads, buildings, power lines).
2. **Match the `object_type` values in the maximum damage file**: Each asset type must have a corresponding entry.
3. **Be linked to vulnerability curves**: These curves define how different asset types respond to specific hazards.
4. **Account for geometry type**: Maximum damages are often linked to some unit (e.g., full object, meters or square meters).

## Sector-Specific Examples

### 1. Transportation Infrastructure: Roads

#### Preparing the Data

A local dataset of road networks may contain different road classifications. The dataset must include an `object_type` column and the geometry type must be verified.

```python
import geopandas as gpd

# Load local road network data
gdf_roads = gpd.read_file("path/to/local_roads.geojson")

# Ensure 'object_type' column exists and is correctly populated
gdf_roads['object_type'] = gdf_roads['road_type']  # e.g., 'primary', 'secondary', 'tertiary'

# Check geometry type
gdf_roads["geometry_type"] = gdf_roads.geometry.type

# Display first few rows
print(gdf_roads.head())
```

#### Creating the Maximum Damage DataFrame

Maximum damage values must be defined based on whether the asset is measured per meter, per object, or by another unit. It is essential to validate these values against expert knowledge or historical data. While estimates can be derived from literature, validation through local expertise is recommended.

```python
import pandas as pd

# Define maximum reconstruction costs
max_damage_data = {
    'object_type': ['primary', 'secondary', 'tertiary'],
    'max_reconstruction_cost_per_meter': [1000, 750, 500]  # Example costs in currency units per meter
}

# Create DataFrame
max_damage_df = pd.DataFrame(max_damage_data).set_index('object_type')

# Display DataFrame
print(max_damage_df)
```

#### Linking Vulnerability Curves

Each infrastructure type must be linked to a vulnerability curve that defines expected damage based on hazard intensity. Vulnerability curves represent the relationship between hazard intensity (e.g., flood depth, wind speed, ground shaking) and the proportion of damage incurred by the asset.

Vulnerability curves are highly hazard-specific. A curve for flooding will differ from one for earthquakes or cyclones. It is advisable to refer to published sources, such as [Nirandjan et al. (2024)](https://nhess.copernicus.org/articles/24/4341/2024/nhess-24-4341-2024-discussion.html), as a starting point to identify relevant curves. However, local validation and consultation are necessary to ensure accuracy for the specific region and hazard type.

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

### 2. Energy Infrastructure: Power Plants

#### Preparing the Data

A dataset of power plants must include the `object_type` column and specify the geometry type.

```python
# Load local power plant data
gdf_power_plants = gpd.read_file("path/to/local_power_plants.geojson")

# Ensure 'object_type' column exists and is correctly populated
gdf_power_plants['object_type'] = gdf_power_plants['fuel_type']  # e.g., 'coal', 'gas', 'nuclear'

# Check geometry type
gdf_power_plants["geometry_type"] = gdf_power_plants.geometry.type

# Display first few rows
print(gdf_power_plants.head())
```

#### Creating the Maximum Damage DataFrame

For power plants, maximum reconstruction cost is typically per object rather than per meter. Just like with road infrastructure, it is important to validate these values with local expertise and empirical data where available.

```python
# Define maximum reconstruction costs
max_damage_data = {
    'object_type': ['coal', 'gas', 'nuclear'],
    'max_reconstruction_cost_per_object': [50000000, 40000000, 100000000]  # in currency units
}

# Create DataFrame
max_damage_df = pd.DataFrame(max_damage_data).set_index('object_type')

# Display DataFrame
print(max_damage_df)
```

#### Linking Vulnerability Curves

Similar to road networks, vulnerability curves for power plants depend on the hazard being analyzed. These curves should be derived from domain-specific research and, where possible, validated by local stakeholders.

```python
# Define vulnerability curves as a DataFrame
damage_curves_data = {
    'coal': [0.0, 0.1, 0.4, 0.7, 1.0],
    'gas': [0.0, 0.2, 0.5, 0.8, 1.0],
    'nuclear': [0.0, 0.05, 0.3, 0.6, 1.0]
}

# Create DataFrame
depth_levels = [0, 1, 2, 3, 4]
damage_curves = pd.DataFrame(damage_curves_data, index=depth_levels)
damage_curves.index.rename('Depth', inplace=True)
damage_curves = damage_curves.astype(np.float32)

# Display DataFrame
print(damage_curves)
```

## Moving forward

Local infrastructure data must be structured correctly to be used in the `damagescanner`. The `object_type` column must align with values in the maximum damage file, and each type must be linked to an appropriate vulnerability curve. Geometry type must also be considered to determine whether impacts are assessed per square meter, per meter, or per object. 

Both maximum damage values and vulnerability curves require validation through local consultation and expert knowledge. While literature can provide an initial reference, adjustments based on empirical observations and stakeholder input improve the accuracy of risk assessments.
