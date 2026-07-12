# Coastal Remote Sensing & SPM, Water Colour Analysis Workflow

A collection of Jupyter Notebooks for downloading, post-processing, masking, and analyzing Sentinel-2 MSI satellite imagery to extract and study satellite-derived optical products: Suspended Particulate Matter (SPM) and Hue Angle. 
This toolkit leverages the **Copernicus Dataspace Ecosystem (CDSE)** for download of TOA satellite products and utilizes output products derived from atmospheric correction **ACOLITE L2W** water-leaving reflectance.

---

## Workflow & Notebook Sequence

To process and analyze the remote sensing data, execute the notebooks in the following sequence:

1. **`01_downloadRSdata.ipynb`**
   * **Purpose:** Authenticates with the Copernicus Dataspace STAC API and downloads Sentinel-2 L1C `.SAFE.zip` raw files matching cloud-cover constraints for your selected geojson Regions of Interest (ROI).
2. **`02_l2wRGB.ipynb`**
   * **Purpose:** Reads Red, Green, and Blue bands from ACOLITE L2W corrected files, masks out invalid pixels using standard flags, and generates true-color/composited RGB products.
3. **`03_flagsl2w.ipynb`**
   * **Purpose:** Inspects and enumerates ACOLITE L2W quality flags to understand the spatial distribution of invalid pixels, clouds, sun glint, and high turbidities.
4. **`04_readingspmlvls.ipynb`**
   * **Purpose:** Compares different Suspended Particulate Matter (SPM) algorithms (including **Novoa 2017**, **Nechad 2010**, and **Nechad 2016**), applies standard L2W masks, and visualizes turbid patterns.
5. **`05_maskingandmonthlystats.ipynb`**
   * **Purpose:** Performs spectral land-masking based on Red, Green, Blue, NIR bands (Vanhellemont & Ruddick, 2018, Appendix A. Supplementary material) and generates monthly composites of SPM, and Hue bands over a monthly time-series, generating composite stats (such as monthly median and maximum SPM concentration).
6. **`06_spm_hue_thresholding.ipynb`**
   * **Purpose:** Performs coastal water segmentations using percentile thresholding techniques on SPM and Hue Angle datasets combined with coastline shapefiles.
7. **`07_spm_hue_thresholdingArea1.ipynb`**
   * **Purpose:** Continuation of `06_`. Focuses SPM and Hue thresholding and segmentation on merged 2-tile area with high SPM values.


---

##  Installation & Setup

Because geospatial packages (such as GDAL, Rasterio, and GeoPandas) require binary bindings, **highly recommend** setting up your Python environment using [Conda](https://docs.anaconda.com/miniconda/) or [Mamba](https://mamba.readthedocs.io/).

### 1. Recreate the Conda Environment
Clone this repository and run the following commands in your terminal:

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/RSprocessing-coastal-analysis.git
cd RSprocessing-coastal-analysis

# Create environment from the provided environment.yml
conda env create -f environment.yml

# Activate the environment
conda activate geo-env
```

### 2. Launch Jupyter Notebooks
```bash
jupyter notebook
```

---

## Repository Directory Structure

Maintain the directory structure below to ensure file paths resolve correctly inside your notebooks:

```text
RSprocessing-coastal-analysis/
├── environment.yml             # Conda package configurations
├── requirements.txt            # Alternative pip package configuration
├── .gitignore                  # Prevents uploading heavy raster files to GitHub
├── README.md                   # This documentation guide
├── ROI/                        # Target Regions of Interest (geojson files)
│   ├── Roi.*                   # geojson example of local ROI
├── notebooks/                  # Sequenced Jupyter Notebooks
│   ├── 01_downloadRSdata.ipynb
│   └── ...
└── data/                       # Directory for downloaded SAFE and processed .tif images (ignored by Git)
```

> **Note:** The `data/` folder is excluded from version control to keep the repository lightweight. When running the notebooks, raw downloads and generated composites will be outputted here.

---

##  Credentials & CDSE Downloads

The raw data downloading notebook (`01_downloadRSdata.ipynb`) requires Copernicus Dataspace Ecosystem credentials. 
* To obtain a free account, sign up at [Copernicus Dataspace Ecosystem](https://dataspace.copernicus.eu/).
* When running the download notebook, you will be prompted securely for your login credentials via standard terminal inputs. No credentials are saved or hardcoded.

---

## References & Algorithms

* **References**

Vanhellemont, Q., Aurin, D., Boettcher, M., Hundt, S., Bakken, S., Voirand, T., & ... (2025). ACOLITE: generic atmospheric correction module (version 20250402.0). Zenodo. https://doi.org/10.5281/zenodo.15125322

Vanhellemont, Q., & Ruddick, K. (2018). Atmospheric correction of metre-scale optical satellite data for inland and coastal water applications. Remote Sensing of Environment, 216(July), 586–597. https://doi.org/10.1016/j.rse.2018.07.015

* **Algorithmic Retrievals (from ACOLITE products)**
  * **SPM by Nechad et al. (2016)**
  * **SPM by Novoa et al. (2017)** 
  * **Hue angle**
  * **SLH by Kudela et. al (2015)**

