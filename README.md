# Time_series_composite
Extracting time series composite of Landsat images using Google Earth Engine

The goal of this script is to help extract a time series composite of Landsat 5 and 7 images over multiple years to be used in developing a deep learning model for classifying land use following deforestation.
Two median composite images will be extracted per year for the months of January-June and July-December. Each composite will contain  7 Landsat bands and an addition of two bands (NDVI na NDMI).

## Landsat image collection
In this project, we used atmospheric corrected Landsat 5 and 7 surface reflectance data. In addition we applied a function for cloud masking to remove cloudy pixels before calculating and adding the NDMI and NDVI bands to the composite.
Hence, for each time step we will be getting 9 bands (7 Landsat bands + 1 NDVI + 1 NDMI).


```python
var geom = ee.FeatureCollection('users/robertnag82/land_use_polygon');
print(geom);
Map.centerObject(geom)

// Function to cloud mask Landsats 4-7
var maskL57SR = function(image) {
  var qa = image.select('pixel_qa');
  // Second bit must be zero, meaning none to low cloud confidence.
  var mask1 = qa.bitwiseAnd(ee.Number(2).pow(7).int()).eq(0).and(
      qa.bitwiseAnd(ee.Number(2).pow(3).int()).lte(0)); // cloud shadow
  // This gets rid of noise at the edge of the images.
  var mask2 = image.select('B.*').gt(0).reduce('min');
  return image
      .select(['B1','B2', 'B3', 'B4', 'B5','B6','B7']).multiply(0.0001)
      .updateMask(mask1.and(mask2));
};

//Abbreviations for composite images for January - June = JJ, July - December =JD

//January - June IMAGERY 2000/////////////////////////////////////////////////////////////////////////////////
var  collection20JJ = ee.ImageCollection('LANDSAT/LE07/C01/T1_SR')
   .merge(ee.ImageCollection('LANDSAT/LT05/C01/T1_SR'))
   .filterBounds(geom)
   .select(['B1','B2', 'B3', 'B4', 'B5','B6','B7','pixel_qa'])
   .filterDate('2000-01-01', '2000-06-30')
   .filter(ee.Filter.lt('CLOUD_COVER', 50))
   .map(maskL57SR)
//   .sort('system:time_start', false)

//Function to get NDVI 
var addNDVI20JJ = function(image) {
var ndvi20JJ = image.normalizedDifference(['B4', 'B3']).rename('NDVI20JJ');
return image.addBands(ndvi20JJ);
};

//Function to get NDMI 
var addNDMI20JJ = function(image) {
var ndmi20JJ = image.normalizedDifference(['B4', 'B5']).rename('NDMI20JJ');
return image.addBands(ndmi20JJ);
};

//Add NDVI bands to image collection
var collectionwithNDVI20JJ = collection20JJ.map(addNDVI20JJ).map(addNDMI20JJ);

print(collectionwithNDVI20JJ);

///July - December 2000
var  collection20JD = ee.ImageCollection('LANDSAT/LE07/C01/T1_SR')
   .merge(ee.ImageCollection('LANDSAT/LT05/C01/T1_SR'))
   .filterBounds(geom)
   .select(['B1','B2', 'B3', 'B4', 'B5','B6','B7','pixel_qa'])
   .filterDate('2000-07-01', '2000-12-30')
   .filter(ee.Filter.lt('CLOUD_COVER', 50))
   .map(maskL57SR)

//Function to get NDVI 
var addNDVI20JD = function(image) {
```
