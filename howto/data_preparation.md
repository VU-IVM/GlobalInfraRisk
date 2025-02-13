# Specific Data Preparations

## Ensuring Consistency Across Geometries

When preparing infrastructure and hazard datasets for risk analysis, it is important to maintain consistency across different geometry types. Some datasets store infrastructure as **points**, **linestrings**, or **polygons**, and these need to be standardized to ensure proper spatial analysis.

For some data, both points and polygons are available. Estimating damage using polygons simplifies the analysis. Therefore, we convert points to polygons by estimating their approximate size and applying buffers. For example, substations in a power network may be stored as lines and points, respectively. To ensure accurate exposure calculations, we convert them to **polygons** using buffer operations.

#### **Step 1: Checking Geometry Types**
Before making modifications, check how different assets are represented in the dataset:

```python
features['geom_type'] = features.geom_type
print(features.groupby(['object_type', 'geom_type']).count()['geometry'])
```

This will display the number of objects stored as **Point**, **LineString**, or **Polygon**, allowing us to identify which assets need conversion.

### **Converting LineStrings to Polygons**
Some assets, such as **power plants, substations, and generators**, are stored as LineStrings but should be polygons. We convert these using the following steps:

#### **Step 1: Define Asset Types for Conversion**
```python
linestrings_to_polygonize = ['generator', 'plant', 'substation']
```
This list specifies which asset types should be converted from **LineStrings to Polygons**.

#### **Step 2: Select Relevant Assets**
```python
all_assets_to_polygonize = features.loc[
    (features.object_type.isin(linestrings_to_polygonize)) & (features.geometry.geom_type == 'LineString')
]
```
This extracts only the LineString geometries belonging to the asset types in our list.

#### **Step 3: Convert LineStrings to Polygons**
```python
import shapely

def polygonize_linestring_per_asset(asset):
    return shapely.Polygon(asset.geometry)

collect_new_linestring_geometries = all_assets_to_polygonize.apply(
    lambda asset: polygonize_linestring_per_asset(asset), axis=1
).values
```
Here, we use **Shapely** to convert each **LineString** into a **Polygon**.

#### **Step 4: Overwrite the Original Geometries**
```python
features.loc[
    (features.object_type.isin(linestrings_to_polygonize)) & (features.geometry.geom_type == 'LineString'),
    'geometry'
] = collect_new_linestring_geometries
```
This replaces the existing **LineString** geometries with the newly created **Polygon** geometries.

### **Converting Points to Polygons**
Some infrastructure elements, such as **substations, generators, and plants**, are stored as **Points** but should have an area representation. We use buffering to convert them into small square polygons.

#### **Step 1: Estimate Average Asset Size**
```python
polygon_features = features.loc[features.geometry.geom_type.isin(['Polygon', 'MultiPolygon'])].to_crs(3857)
polygon_features['square_m2'] = polygon_features.area

square_m2_object_type = polygon_features[['object_type', 'square_m2']].groupby('object_type').median()
```
Here, we calculate the **median area** of existing polygons for each asset type, which will be used to determine the buffer size for point geometries.

#### **Step 2: Define Asset Types for Buffering**
```python
points_to_polygonize = ['generator', 'plant', 'substation']
```
Only these assets will be converted from **Points to Polygons**.

#### **Step 3: Select Relevant Points**
```python
all_assets_to_polygonize = features.loc[
    (features.object_type.isin(points_to_polygonize)) & (features.geometry.geom_type == 'Point')
]
all_assets_to_polygonize = all_assets_to_polygonize.to_crs(3857)
```
This extracts the relevant point geometries and temporarily converts them to a coordinate reference system that uses **meters** (EPSG:3857), allowing for proper buffering.

#### **Step 4: Apply Buffering to Convert Points into Polygons**
```python
import numpy as np

def polygonize_point_per_asset(asset):
    buffer_length = np.sqrt(square_m2_object_type.loc[asset.object_type].values[0]) / 2
    return asset.geometry.buffer(buffer_length, cap_style='square')

collect_new_point_geometries = all_assets_to_polygonize.apply(
    lambda asset: polygonize_point_per_asset(asset), axis=1
).set_crs(3857).to_crs(4326).values
```
This function calculates the **buffer distance** based on the estimated asset size and applies it to each point.

#### **Step 5: Overwrite the Original Geometries**
```python
features.loc[
    (features.object_type.isin(points_to_polygonize)) & (features.geometry.geom_type == 'Point'),
    'geometry'
] = collect_new_point_geometries
```
This replaces the original **Point** geometries with **Polygon** representations.

#### **Step 6: Clean Up Temporary Data**
```python
features = features.drop(['geom_type'], axis=1)
```
Removing unnecessary columns ensures that the dataset remains clean for further analysis.

---

## Checking Coordinate Reference Systems (CRS)

Geospatial data is often provided in different coordinate reference systems (CRS). The **default CRS for most global datasets is EPSG:4326** ([WGS 84](https://epsg.io/4326)), which represents coordinates in degrees. However, many local datasets use **projected coordinate systems** (e.g., UTM or national grids), which use meters instead.

To ensure consistency, all datasets should be transformed to the same CRS before running **DamageScanner**.

```python
# Check CRS of the dataset
gdf_power.crs

# Convert to EPSG:4326 if necessary
gdf_power = gdf_power.to_crs(epsg=4326)
```

Ensuring the same CRS across all input datasets prevents misalignment issues when overlaying hazard and exposure data.

---

## Optimizing Data for Computational Efficiency

While the code is optimized, running the analysis on large datasets can be resource-intensive. To improve efficiency:

- **Use the smallest necessary spatial extent**
- **Clip datasets to the area of interest** before running calculations
- **Resample high-resolution hazard datasets** when applicable

### **Example: Clipping Hazard Data to the Country of Interest**

```python
import xarray as xr
import rioxarray as rioxr

# Load global hazard dataset
hazard_map = xr.open_dataset("https://hazards-data.unepgrid.ch/global_pc_h100glob.tif", engine="rasterio")

# Load country boundary data
world = gpd.read_file("world_boundaries.geojson")
country_iso3 = "NLD"  # Example: Netherlands

# Extract country boundary
country_bounds = world.loc[world.ADM0_ISO == country_iso3].bounds

# Clip hazard data to country extent
hazard_country = hazard_map.rio.clip_box(
    minx=country_bounds.minx.values[0],
    miny=country_bounds.miny.values[0],
    maxx=country_bounds.maxx.values[0],
    maxy=country_bounds.maxy.values[0]
)
```

By reducing the spatial extent, we improve performance and avoid unnecessary computations on large datasets. Ensuring consistency in **geometry types**, **coordinate systems**, and **data size** is crucial for accurate and efficient risk assessment workflows.
