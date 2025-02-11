# Run Locally
This guide provides step-by-step instructions for setting up and running the Jupyter notebooks included in the Global Infrastructure Risk repository. By following these instructions, you will be able to configure a reproducible computing environment using Conda and interact with the provided notebooks efficiently. 

## Prerequisites

Ensure you have the following installed:

- [Miniconda/Anaconda](https://docs.conda.io/en/latest/)
- [Git](https://git-scm.com/)

## Clone the Repository

To get started, clone the repository from GitHub and navigate into the project directory:

```bash
git clone https://github.com/VU-IVM/GlobalInfraRisk.git
cd GlobalInfraRisk
```

## Setting Up the Conda Environment

To install all necessary dependencies, create and activate the Conda environment using the provided `environment.yml` file:

```bash
conda env create -f environment.yml
conda activate infra-risk
```

## Running the Notebooks

Once the environment is activated, launch JupyterLab to interact with the notebooks:

```bash
jupyter lab
```

This will open JupyterLab in your browser. Navigate to the desired notebook and start running it!

## Environment File Contents and Dependencies

The `environment.yml` file specifies the dependencies required to run the notebooks. Below is the content of the file:

```yaml
name: infra-risk
channels:
  - conda-forge
dependencies:
  - python=3.12
  - numpy
  - geopandas
  - rasterio
  - matplotlib
  - tqdm
  - pip
  - jupyterlab
  - pyproj
  - xarray
  - rioxarray
  - pip:
      - damagescanner==0.9b12
      - exactextract
      - contextily
      - openpyxl
      - pyarrow
      - lonboard
```

### Explanation of Dependencies

- **Python 3.12**: Core programming language.
- **numpy**: Essential for numerical computations.
- **geopandas**: Enables spatial data handling.
- **rasterio**: Supports geospatial raster data operations.
- **matplotlib**: For generating plots and visualizations.
- **tqdm**: Progress bar utility for data processing.
- **pip**: Installs and manages Python packages.
- **jupyterlab**: Web-based environment for interactive computing.
- **xlrd**: Reads Excel files.
- **pyproj**: Performs coordinate transformations.
- **xarray**: Supports multi-dimensional labeled datasets.
- **rioxarray**: Extends xarray for raster data.
- **damagescanner**: Risk assessment tool.
- **exactextract**: Extracts zonal statistics from raster data.
- **contextily**: Adds basemaps to geospatial visualizations.
- **openpyxl**: Reads and writes Excel files.
- **pyarrow**: Handles in-memory columnar data.
- **lonboard**: Custom package (verify availability if needed).

