# Hazard & Hazard Data

Hazards refer to potentially damaging physical events, phenomena, or human activities that can lead to the destruction or degradation of critical infrastructure (CI). These hazards can be natural, such as floods and earthquakes, or human-made, such as industrial accidents and cyber-attacks. Understanding the range and intensity of hazards that infrastructure is exposed to is a crucial step in assessing risks and ensuring resilience.

## Types of Hazards
Natural hazards are caused by environmental factors and include events that occur due to geological, meteorological, and climatic processes. These can lead to widespread damage and disruption to CI. Common natural hazards include:

- **Flooding**: Both river and surface flooding are common natural hazards affecting CI. Flooding can cause road damage, electrical outages, and impede access to health facilities. Surface flooding occurs when intense rainfall overwhelms drainage systems, while river flooding occurs when rivers overflow their banks. Coastal flooding can also occur due to storm surges and rising sea levels.
  
- **Earthquakes**: Seismic activity can cause infrastructure failure, especially in areas where roads, bridges, and buildings are not designed to withstand strong ground shaking. Earthquakes can also trigger secondary hazards like landslides and tsunamis.

- **Tropical Cyclones**: These storms are accompanied by strong winds and heavy rains, causing damage to electrical grids, transportation networks, and communication systems. The high wind speeds can knock down power lines and communication towers, while storm surges can cause coastal flooding.

- **Landslides**: These hazards occur when unstable slopes fail, usually triggered by heavy rainfall or seismic activity. Landslides can destroy transportation routes, power lines, and water supply systems.

- **Tsunamis**: Tsunamis are caused by underwater earthquakes, volcanic eruptions, or landslides. They can lead to widespread flooding, destroying coastal infrastructure and disrupting essential services.

- **Heatwaves**: Prolonged periods of extreme heat can lead to infrastructure failures, such as power outages due to increased demand for cooling systems. Heatwaves can also affect transportation networks, causing railway tracks to warp and roads to soften, leading to dangerous driving conditions.

- **Wildfires**: Wildfires, often triggered by droughts and extreme heat, can severely damage infrastructure. These fires can destroy power lines, telecommunications towers, and transportation routes, as well as disrupt water and fuel supplies. In areas prone to wildfires, critical infrastructure must be designed to withstand or mitigate the risks of fire.

---

## Hazard Intensity and Return Periods

The severity and frequency of hazards are often expressed through **intensity** and **return periods**. Intensity refers to the magnitude or severity of the hazard event, while return periods refer to the likelihood of the event occurring over a specified time frame (e.g., a 1-in-100-year flood).

### Hazard Mapping
Hazard mapping is used to visually represent areas that are prone to specific hazards, such as floodplains, seismic zones, or areas frequently hit by tropical storms. These maps are critical tools in risk assessment as they help identify which CI systems are most vulnerable.

### Hazard Intensity Bands
Hazards are categorized into intensity bands to estimate potential damage. For example, earthquake intensity is measured by peak ground acceleration (PGA), while wind speeds are used for cyclones. Flood hazard intensity is typically measured in terms of water depth.

## Publicly Available Hazard Data

Various global datasets provide hazard information that can be used for risk assessments. These datasets offer modeled hazard maps for different natural hazards, such as flooding, tropical cyclones, and earthquakes. Below, we provide an overview of available datasets along with links to access and process the data. All these datasets should work directly with the tools provided within the notebooks and examples.

### **Global Flood Hazard Data**
Flood hazard data includes riverine and coastal flood models that estimate inundation depths and extents for different return periods. Pluvial flooding is generally only available through some of the commercial flood model companies (e.g. [**Fathom**](https://www.fathom.global/) & [**JBA**](https://www.jbarisk.com/products/global-flood-maps/))

```{important}
The list provided below is specifically focused on global datasets. These datasets are often characterized by coarser resolulations and model simplifications. Only use them when regionally refined information is unavailable, and interpret the results with this data cautiously.
```

- **[CDRI - River Flooding](https://giri.unepgrid.ch/map?list=explore)**  
  The Climate and Disaster Resilience Initiative (CDRI) provides global river flood hazard maps.  
  📄 [Background Report](https://giri.unepgrid.ch/sites/default/files/2023-09/CIMA_GIRI_Flood_BGpaper_Supplement.pdf)

- **[JRC Global River Flood Maps](https://data.jrc.ec.europa.eu/dataset/jrc-floods-floodmapgl_rp50y-tif)**  
  The Joint Research Centre (JRC) offers global flood maps based on GLOFAS, available for different return periods.  
  📥 [Download Data](https://jeodpp.jrc.ec.europa.eu/ftp/jrc-opendata/CEMS-GLOFAS/flood_hazard/)

- **[Aqueduct Flood Hazard Maps](https://www.wri.org/data/aqueduct-floods-hazard-maps)**  
  Aqueduct provides river and coastal flood hazard maps with projections under different climate scenarios.  
  📥 [Download Data](https://wri-projects.s3.amazonaws.com/AqueductFloodTool/download/v2/index.html)

### **Global Tropical Cyclone Hazard Data**
Cyclone hazard data includes present and future projections of tropical cyclone wind speeds.

- **[STORM Tropical Cyclone Hazard Data](https://data.4tu.nl/datasets/0ea98bdd-5772-4da8-ae97-99735e891aff/4)**  
  Provides modeled tropical cyclone wind speeds for the present-day climate. Future projections are available [here](https://data.4tu.nl/datasets/504c838e-2bd8-4d61-85a1-d495bdc560c3/4).  
  📄 [Data Explanation](https://www.nature.com/articles/s41597-020-0381-2)

- **[CDRI - Tropical Cyclone Maps](https://giri.unepgrid.ch/map?list=explore)**  
  CDRI provides present-day and future cyclone hazard maps.  
  📄 [Background Report](https://giri.unepgrid.ch/sites/default/files/2023-11/2.4-INGENIAR-CDRI-Background-Report-Risk-model.pdf)

### **Global Earthquake Hazard Data**
Earthquake hazard data includes seismic hazard maps that estimate ground shaking levels based on past earthquake records and geological modeling.

- **[CDRI - Earthquake Hazard Maps](https://giri.unepgrid.ch/map?list=explore)**  
  The Climate and Disaster Resilience Initiative (CDRI) provides global earthquake hazard maps.  
  📄 [Background Report](https://giri.unepgrid.ch/sites/default/files/2023-11/2.4-INGENIAR-CDRI-Background-Report-Risk-model.pdf)

- **[Global Earthquake Model (GEM)](https://www.globalquakemodel.org/product/global-seismic-hazard-map)**  
  GEM provides a probabilistic seismic hazard assessment with global coverage.  
  📥 [Download Data](https://cloud.openquake.org/s/6SnFk2f92JEr76H)

- **[GAR 2017 Atlas Risk Data](https://risk.preventionweb.net/)**  
  The Global Assessment Report (GAR) provides earthquake hazard layers at the global level.

### **Global Landslide Hazard Data**
For Landslides, there is limited information available and are generally more difficult to be used directly in any damage or risk assessment. 

- **[CDRI - Landslide Hazard Data](https://giri.unepgrid.ch/map?list=explore)**  
  The Climate and Disaster Resilience Initiative (CDRI) provides global landslide hazard maps for both rainfall-triggered and earthquake-triggered landslides. Rainfall-induced landslide models are available for both present and future climate conditions.  
  📄 [Background Report](https://giri.unepgrid.ch/sites/default/files/2023-06/20230615-NGI_manuscript_GIRI_landlside_hazard_model.pdf)

- **[World Bank Global Landslide Hazard Maps](https://jkan.riskdatalibrary.org/datasets/global-landslide-hazard-maps/)**  
  Provides median and mean hazard maps for landslides triggered by both heavy rainfall and earthquakes.  
  📥 [Median Rainfall Landslide Hazard (1980-2018)](https://datacatalogfiles.worldbank.org/ddh-published/0037584/DR0045414/ls_rf_median_1980-2018.zip)  
  📥 [Mean Rainfall Landslide Hazard (1980-2018)](https://datacatalogfiles.worldbank.org/ddh-published/0037584/DR0045413/ls_rf_mean_1980-2018.zip)  
  📥 [Median Earthquake-Triggered Landslide Hazard](https://datacatalogfiles.worldbank.org/ddh-published/0037584/DR0045412/ls_eq.zip)

### **Global Wildfire Hazard Data**
For wildfires, there are no globally available hazard maps with specific return periods. Most wildfire hazard modeling is done on a case-by-case basis. The dataset used in the examples is:

- **[GlobFire Fire Perimeters (2002 - 2023)](https://effis-gwis-cms.s3.eu-west-1.amazonaws.com/apps/country.profile/GLOBFIRE_burned_area_full_dataset_2002_2023.zip)**  
  A global dataset of individual fire perimeters from 2002-2023, derived from the MCD64A1 burned area product. The dataset includes unique fire identification codes, initial and final dates, fire geometry, and final area in hectares.  
  📄 [Data Description](https://doi.org/10.1038/s41597-019-0312-2)

### **Global Temperature Data**

Temperature data is crucial for understanding climate change impacts on infrastructure systems. Various global sources provide temperature datasets, with the **latest climate change projections** developed under the [CMIP6](https://wcrp-cmip.org/cmip6/) initiative. This data has been downscaled through multiple initiatives to improve regional applicability. 

```{note}
A key challenge with temperature datasets is the **large volume of data** generated, particularly for high-resolution daily records.
```

#### **High-Resolution Daily Global Climate Dataset**
- **[CEDA High-Resolution Climate Data](https://catalogue.ceda.ac.uk/uuid/c107618f1db34801bb88a1e927b82317/)**  
  Provides **daily global climate data** at a **0.25-degree horizontal resolution** for both **historical (1981-2014)** and **future (2015-2100)** climate projections. The dataset includes climate simulations under three Shared Socioeconomic Pathways (**SSP2-4.5, SSP5-3.4OS, and SSP5-8.5**).  
  📄 [Dataset Description](https://www.nature.com/articles/s41597-023-02528-x)

#### **CHC-CMIP6: Climate Projection Data with a Focus on Heat-Related Extremes**
- **[CHC-CMIP6 Climate Data](https://www.chc.ucsb.edu/data/chc-cmip6)**  
  A **high-resolution global climate dataset** specifically designed for analyzing **heat-related hydroclimatic extremes**. This dataset includes **daily gridded data (0.05° resolution)** for both **observational (1983-2016)** and **projection periods (2030 and 2050)**.  
  📄 [Dataset Details](https://www.nature.com/articles/s41597-024-03074-w)  
  📥 [Download Data](https://www.chc.ucsb.edu/data/chc-cmip6)


## Example: Reading Flood Hazard Data in Python
```python
import xarray as xr

# Load flood hazard data from UNEP-GRID
flood_ds = xr.open_dataset("https://hazards-data.unepgrid.ch/global_pc_h100glob.tif", engine="rasterio")

# Display dataset information
print(flood_ds)
```
