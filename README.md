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
var ndvi20JD = image.normalizedDifference(['B4', 'B3']).rename('NDVI20JD');
return image.addBands(ndvi20JD);
};

//Function to get NDMI 
var addNDMI20JD = function(image) {
var ndmi20JD = image.normalizedDifference(['B4', 'B5']).rename('NDMI20JD');
return image.addBands(ndmi20JD);
};

//Add NDVI bands to image collection
var collectionwithNDVI20JD = collection20JD.map(addNDVI20JD).map(addNDMI20JD);
print(collectionwithNDVI20JD);

//January - June IMAGERY 2001/////////////////////////////////////////////////////////////////////////////////
var  collection01JJ = ee.ImageCollection('LANDSAT/LE07/C01/T1_SR')
   .merge(ee.ImageCollection('LANDSAT/LT05/C01/T1_SR'))
   .filterBounds(geom)
   .select(['B1','B2', 'B3', 'B4', 'B5','B6','B7','pixel_qa'])
   .filterDate('2001-01-01', '2001-06-30')
   .filter(ee.Filter.lt('CLOUD_COVER', 50))
   .map(maskL57SR)

//Function to get NDVI 
var addNDVI01JJ = function(image) {
var ndvi01JJ = image.normalizedDifference(['B4', 'B3']).rename('NDVI01JJ');
return image.addBands(ndvi01JJ);
};

//Function to get NDVI 
var addNDMI01JJ = function(image) {
var ndmi01JJ = image.normalizedDifference(['B4', 'B5']).rename('NDMI01JJ');
return image.addBands(ndmi01JJ);
};

//Add NDVI bands to image collection
var collectionwithNDVI01JJ = collection01JJ.map(addNDVI01JJ).map(addNDMI01JJ);
print(collectionwithNDVI01JJ);

///July - December 2001
var  collection01JD = ee.ImageCollection('LANDSAT/LE07/C01/T1_SR')
   .merge(ee.ImageCollection('LANDSAT/LT05/C01/T1_SR'))
   .filterBounds(geom)
   .select(['B1','B2', 'B3', 'B4', 'B5','B6','B7','pixel_qa'])
   .filterDate('2001-07-01', '2001-12-30')
   .filter(ee.Filter.lt('CLOUD_COVER', 50))
   .map(maskL57SR)

//Function to get NDVI 
var addNDVI01JD = function(image) {
var ndvi01JD = image.normalizedDifference(['B4', 'B3']).rename('NDVI01JD');
return image.addBands(ndvi01JD);
};

//Function to get NDMI 
var addNDMI01JD = function(image) {
var ndmi01JD = image.normalizedDifference(['B4', 'B5']).rename('NDMI01JD');
return image.addBands(ndmi01JD);
};

//Add NDVI bands to image collection
var collectionwithNDVI01JD = collection01JD.map(addNDVI01JD).map(addNDMI01JD);
print(collectionwithNDVI01JD);


//January - June IMAGERY 2002/////////////////////////////////////////////////////////////////////////////////
var  collection02JJ = ee.ImageCollection('LANDSAT/LE07/C01/T1_SR')
   .merge(ee.ImageCollection('LANDSAT/LT05/C01/T1_SR'))
   .filterBounds(geom)
   .select(['B1','B2', 'B3', 'B4', 'B5','B6','B7','pixel_qa'])
   .filterDate('2002-01-01', '2002-06-30')
   .filter(ee.Filter.lt('CLOUD_COVER', 50))
   .map(maskL57SR)
  .map(function(image){
      var filled1a = image.focal_mean(2, 'square', 'pixels', 1)
      return filled1a.blend(image);
  });

//Function to get NDVI 
var addNDVI02JJ = function(image) {
var ndvi02JJ = image.normalizedDifference(['B4', 'B3']).rename('NDVI02JJ');
return image.addBands(ndvi02JJ);
};

//Function to get NDMI 
var addNDMI02JJ = function(image) {
var ndmi02JJ = image.normalizedDifference(['B4', 'B5']).rename('NDMI02JJ');
return image.addBands(ndmi02JJ);
};

//Add NDVI bands to image collection
var collectionwithNDVI02JJ = collection02JJ.map(addNDVI02JJ).map(addNDMI02JJ);
print(collectionwithNDVI02JJ);

///July - December 2002
var  collection02JD = ee.ImageCollection('LANDSAT/LE07/C01/T1_SR')
   .merge(ee.ImageCollection('LANDSAT/LT05/C01/T1_SR'))
   .filterBounds(geom)
   .select(['B1','B2', 'B3', 'B4', 'B5','B6','B7','pixel_qa'])
   .filterDate('2002-07-01', '2002-12-30')
   .filter(ee.Filter.lt('CLOUD_COVER', 50))
   .map(maskL57SR)
  .map(function(image){
      var filled1a = image.focal_mean(2, 'square', 'pixels', 1)
      return filled1a.blend(image);
  });

//Function to get NDVI 
var addNDVI02JD = function(image) {
var ndvi02JD = image.normalizedDifference(['B4', 'B3']).rename('NDVI02JD');
return image.addBands(ndvi02JD);
};

//Function to get NDMI 
var addNDMI02JD = function(image) {
var ndmi02JD = image.normalizedDifference(['B4', 'B5']).rename('NDMI02JD');
return image.addBands(ndmi02JD);
};

//Add NDVI bands to image collection
var collectionwithNDVI02JD = collection02JD.map(addNDVI02JD).map(addNDMI02JD);
print(collectionwithNDVI02JD);


//January - June IMAGERY 2003/////////////////////////////////////////////////////////////////////////////////
var  collection03JJ = ee.ImageCollection('LANDSAT/LE07/C01/T1_SR')
   .merge(ee.ImageCollection('LANDSAT/LT05/C01/T1_SR'))
   .filterBounds(geom)
   .select(['B1','B2', 'B3', 'B4', 'B5','B6','B7','pixel_qa'])
   .filterDate('2003-01-01', '2003-06-30')
   .filter(ee.Filter.lt('CLOUD_COVER', 50))
   .map(maskL57SR)
  .map(function(image){
      var filled1a = image.focal_mean(2, 'square', 'pixels', 1)
      return filled1a.blend(image);
  });

//Function to get NDVI 
var addNDVI03JJ = function(image) {
var ndvi03JJ = image.normalizedDifference(['B4', 'B3']).rename('NDVI03JJ');
return image.addBands(ndvi03JJ);
};

//Function to get NDMI 
var addNDMI03JJ = function(image) {
var ndmi03JJ = image.normalizedDifference(['B4', 'B5']).rename('NDMI03JJ');
return image.addBands(ndmi03JJ);
};

//Add NDVI bands to image collection
var collectionwithNDVI03JJ = collection03JJ.map(addNDVI03JJ).map(addNDMI03JJ);
print(collectionwithNDVI03JJ);

///July - December 2003
var  collection03JD = ee.ImageCollection('LANDSAT/LT05/C01/T1_SR')
   .merge(ee.ImageCollection('LANDSAT/LE07/C01/T1_SR'))
   .filterBounds(geom)
   .select(['B1','B2', 'B3', 'B4', 'B5','B6','B7','pixel_qa'])
   .filterDate('2003-07-01', '2003-12-30')
   .filter(ee.Filter.lt('CLOUD_COVER', 50))
   .map(maskL57SR)
  .map(function(image){
      var filled1a = image.focal_mean(2, 'square', 'pixels', 1)
      return filled1a.blend(image);
  });

//Function to get NDVI 
var addNDVI03JD = function(image) {
var ndvi03JD = image.normalizedDifference(['B4', 'B3']).rename('NDVI03JD');
return image.addBands(ndvi03JD);
};

//Function to get NDVI 
var addNDMI03JD = function(image) {
var ndmi03JD = image.normalizedDifference(['B4', 'B5']).rename('NDMI03JD');
return image.addBands(ndmi03JD);
};

//Add NDVI bands to image collection
var collectionwithNDVI03JD = collection03JD.map(addNDVI03JD).map(addNDMI03JD);
print(collectionwithNDVI03JD);


//January - June IMAGERY 2004/////////////////////////////////////////////////////////////////////////////////
var  collection04JJ = ee.ImageCollection('LANDSAT/LT05/C01/T1_SR')
   .merge(ee.ImageCollection('LANDSAT/LE07/C01/T1_SR'))
   .filterBounds(geom)
   .select(['B1','B2', 'B3', 'B4', 'B5','B6','B7','pixel_qa'])
   .filterDate('2004-01-01', '2004-06-30')
   .filter(ee.Filter.lt('CLOUD_COVER', 50))
   .map(maskL57SR)
  .map(function(image){
      var filled1a = image.focal_mean(2, 'square', 'pixels', 1)
      return filled1a.blend(image);
  });

//Function to get NDVI 
var addNDVI04JJ = function(image) {
var ndvi04JJ = image.normalizedDifference(['B4', 'B3']).rename('NDVI04JJ');
return image.addBands(ndvi04JJ);
};

//Function to get NDMI 
var addNDMI04JJ = function(image) {
var ndmi04JJ = image.normalizedDifference(['B4', 'B5']).rename('NDMI04JJ');
return image.addBands(ndmi04JJ);
};

//Add NDVI bands to image collection
var collectionwithNDVI04JJ = collection04JJ.map(addNDVI04JJ).map(addNDMI04JJ);
print(collectionwithNDVI04JJ);

///July - December 2004
var  collection04JD = ee.ImageCollection('LANDSAT/LT05/C01/T1_SR')
   .merge(ee.ImageCollection('LANDSAT/LE07/C01/T1_SR'))
   .filterBounds(geom)
   .select(['B1','B2', 'B3', 'B4', 'B5','B6','B7','pixel_qa'])
   .filterDate('2004-07-01', '2004-12-30')
   .filter(ee.Filter.lt('CLOUD_COVER', 50))
   .map(maskL57SR)
  .map(function(image){
      var filled1a = image.focal_mean(2, 'square', 'pixels', 1)
      return filled1a.blend(image);
  });

//Function to get NDVI 
var addNDVI04JD = function(image) {
var ndvi04JD = image.normalizedDifference(['B4', 'B3']).rename('NDVI04JD');
return image.addBands(ndvi04JD);
};

//Function to get NDMI 
var addNDMI04JD = function(image) {
var ndmi04JD = image.normalizedDifference(['B4', 'B5']).rename('NDMI04JD');
return image.addBands(ndmi04JD);
};

//Add NDVI bands to image collection
var collectionwithNDVI04JD = collection04JD.map(addNDVI04JD).map(addNDMI04JD);
print(collectionwithNDVI04JD);


//January - June IMAGERY 2005/////////////////////////////////////////////////////////////////////////////////
var  collection05JJ = ee.ImageCollection('LANDSAT/LT05/C01/T1_SR')
   .merge(ee.ImageCollection('LANDSAT/LE07/C01/T1_SR'))
   .sort("system:time_start")    // Sort by Date
   .filterBounds(geom)
   .select(['B1','B2', 'B3', 'B4', 'B5','B6','B7','pixel_qa'])
   .filterDate('2005-01-01', '2005-06-30')
   .filter(ee.Filter.lt('CLOUD_COVER', 50))
   .map(maskL57SR)
  .map(function(image){
      var filled1a = image.focal_mean(2, 'square', 'pixels', 1)
      return filled1a.blend(image);
  });

//Function to get NDVI 
var addNDVI05JJ = function(image) {
var ndvi05JJ = image.normalizedDifference(['B4', 'B3']).rename('NDVI05JJ');
return image.addBands(ndvi05JJ);
};

//Function to get NDMI 
var addNDMI05JJ = function(image) {
var ndmi05JJ = image.normalizedDifference(['B4', 'B5']).rename('NDMI05JJ');
return image.addBands(ndmi05JJ);
};

//Add NDVI bands to image collection
var collectionwithNDVI05JJ = collection05JJ.map(addNDVI05JJ).map(addNDMI05JJ);
print(collectionwithNDVI05JJ);

///July - December 2005
var  collection05JD = ee.ImageCollection('LANDSAT/LT05/C01/T1_SR')
   .merge(ee.ImageCollection('LANDSAT/LE07/C01/T1_SR'))
   .sort("system:time_start")    // Sort by Date
   .filterBounds(geom)
   .select(['B1','B2', 'B3', 'B4', 'B5','B6','B7','pixel_qa'])
   .filterDate('2005-07-01', '2005-12-30')
   .filter(ee.Filter.lt('CLOUD_COVER', 50))
   .map(maskL57SR)
  .map(function(image){
      var filled1a = image.focal_mean(2, 'square', 'pixels', 1)
      return filled1a.blend(image)
  });
//   .select(bands);

//Function to get NDVI 
var addNDVI05JD = function(image) {
var ndvi05JD = image.normalizedDifference(['B4', 'B3']).rename('NDVI05JD');
return image.addBands(ndvi05JD);
};

//Function to get NDMI 
var addNDMI05JD = function(image) {
var ndmi05JD = image.normalizedDifference(['B4', 'B5']).rename('NDMI05JD');
return image.addBands(ndmi05JD);
};

//Add NDVI bands to image collection
var collectionwithNDVI05JD = collection05JD.map(addNDVI05JD).map(addNDMI05JD);
print(collectionwithNDVI05JD);



///so far this is finding all the images in the collection which meets the criteria- the latest on top. To get a nice blended- looking mosaic and composite,
///we try some of the tools for 'reducing' these to one pixel (or bands of pixels in a layer stack).

//var medianpixels = collection.median() // This finds the mean value of all pixels which meet the criteria.

//var medianpixelsclipped = medianpixels.clip(geom).divide(10000) //this cuts up the result so that it fits neatly into our 
                                                                //aoi and divide so that values are between 0 and 1.

var L5Mosaic20JJ = collectionwithNDVI20JJ.median().clip(geom);
var L5Mosaic20JD = collectionwithNDVI20JD.median().clip(geom);
var L5Mosaic01JJ = collectionwithNDVI01JJ.median().clip(geom);
var L5Mosaic01JD = collectionwithNDVI01JD.median().clip(geom);
var L5Mosaic02JJ = collectionwithNDVI02JJ.median().clip(geom);
var L5Mosaic02JD = collectionwithNDVI02JD.median().clip(geom);
var L5Mosaic03JJ = collectionwithNDVI03JJ.median().clip(geom);
var L5Mosaic03JD = collectionwithNDVI03JD.median().clip(geom);
var L5Mosaic04JJ = collectionwithNDVI04JJ.median().clip(geom);
var L5Mosaic04JD = collectionwithNDVI04JD.median().clip(geom);
var L5Mosaic05JJ = collectionwithNDVI05JJ.median().clip(geom);
var L5Mosaic05JD = collectionwithNDVI05JD.median().clip(geom);

//Stack the median composite into a collection
var stackedL_FLU= (L5Mosaic20JJ.addBands(L5Mosaic20JD).addBands(L5Mosaic01JJ).addBands(L5Mosaic01JD)
.addBands(L5Mosaic02JJ).addBands(L5Mosaic02JD).addBands(L5Mosaic03JJ).addBands(L5Mosaic03JD).addBands(L5Mosaic04JJ)
.addBands(L5Mosaic04JD).addBands(L5Mosaic05JJ).addBands(L5Mosaic05JD).float());

print('stackedL_FLU', stackedL_FLU.bandNames());


//Visualising all composite images in a collection
var ndvi1=stackedL_FLU.select('NDVI05JD')
var ndmi1=stackedL_FLU.select('NDMI05JD')
//print(ndvi1) 
var palette = {min: -1, max: 1, palette: ['blue', 'white', 'green']};
Map.addLayer(ndvi1, palette);
Map.addLayer(ndmi1, palette);
//Now visualize the composite as a natural color image

Map.addLayer(L5Mosaic20JD, {bands: ['B3', 'B2', 'B1'], min: 0, max: 0.2}, 'Landsat_20JD mosaic');
Map.addLayer(L5Mosaic20JJ, {bands: ['B3', 'B2', 'B1'], min: 0, max: 0.2}, 'Landsat_20JJ mosaic');
Map.addLayer(L5Mosaic01JD, {bands: ['B3', 'B2', 'B1'], min: 0, max: 0.2}, 'Landsat_01JD mosaic');
Map.addLayer(L5Mosaic01JJ, {bands: ['B3', 'B2', 'B1'], min: 0, max: 0.2}, 'Landsat_O1JJ mosaic');
Map.addLayer(L5Mosaic02JD, {bands: ['B3', 'B2', 'B1'], min: 0, max: 0.2}, 'Landsat_02JD mosaic');
Map.addLayer(L5Mosaic02JJ, {bands: ['B3', 'B2', 'B1'], min: 0, max: 0.2}, 'Landsat_O2JJ mosaic');
Map.addLayer(L5Mosaic03JD, {bands: ['B3', 'B2', 'B1'], min: 0, max: 0.2}, 'Landsat_03JD mosaic');
Map.addLayer(L5Mosaic03JJ, {bands: ['B3', 'B2', 'B1'], min: 0, max: 0.2}, 'Landsat_O3JJ mosaic');
Map.addLayer(L5Mosaic04JD, {bands: ['B3', 'B2', 'B1'], min: 0, max: 0.2}, 'Landsat_04JD mosaic');
Map.addLayer(L5Mosaic04JJ, {bands: ['B3', 'B2', 'B1'], min: 0, max: 0.2}, 'Landsat_O4JJ mosaic');
Map.addLayer(L5Mosaic05JD, {bands: ['B3', 'B2', 'B1'], min: 0, max: 0.2}, 'Landsat_05JD mosaic');
Map.addLayer(L5Mosaic05JJ, {bands: ['B3', 'B2', 'B1'], min: 0, max: 0.2}, 'Landsat_O5JJ mosaic');


//export it to googledrive as tiff for further analysis
//Export the image specifying scale and region
Export.image.toDrive({
  image:stackedL_FLU,
  description:'stackedL_composites_of_FLU',
  scale: 30,
  maxPixels: 1e9,
  region: geom                                                                                                                                                                                                                                                                                           
```
