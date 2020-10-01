# Time_series_composite
Extracting time series composite of Landsat images using Google Earth Engine

The goal of this script is to help extract a time series composite of Landsat 5 and 7 images over multiple years to be used in developing a deep learning model for classifying land use following deforestation.
Two median composite images will be extracted per year for the months of January-June and July-December. Each composite will contain  7 Landsat bands and an addition of two bands (NDVI na NDMI).

## Landsat image collection
In this project, we used atmospheric corrected Landsat 5 and 7 surface reflectance data. In addition we applied a function for cloud masking to remove cloudy pixels before calculating and adding the NDMI and NDVI bands to the composite.
Hence, for each time step we will be getting 9 bands (7 Landsat bands + 1 NDVI + 1 NDMI).
