# 3.0. Digitization: Burial Mounds in Uedemer Hochwald (Kshama Guari)
## Sub-Task 3.1:Georeference the picture of the map of burial mounds onto a topographic map layer. 

### Workflow 3.1:
- The repository EE_3.07_Geodata_WS2021 was cloned from the provided repository “https://github.com/rolfbecker/EE_3.07_Geodata_WS2021 “
- The file 'gdms0000_Burial_Mounds_Uedem_V001.qgz' was opened on qgis. In it already the map of NRW was uploaded using the WMS named as DTK10.
- The topographic map was then added to the map as a raster layer. Each point on the topographic map was the georeferenced on the DTK10 map.
- The final map after georeferencing was uploaded and the image was cropped to fit by adding 0 additional no data value.


### Detailed steps taken: 
Open the qgis project gdms0000_Burial_Mounds_Uedem_V001.qgz>> Click on the DTK10 layer>> Open the georeferencer from the raster tools>> Load the image file “Burial_Mounds” on the georeferencer >>Add points on the image and referenced it to corresponding points on the DTK10 map>> Saved the GCP as “Burial_Mounds_Points” in the Data/Derived/Georeference”>> Click on “Start Georeferencing” and saved the layer as Burial_mound_georeferenced as a tif file in the same folder >> Loaded the tif layer on the map>>Added 0 as additional data in properties to crop the image to fit the map.

## Sub-Task 3.2:To create a hillshade model from the DTM layer 

### Workflow 3.2 
-  Work is continued on the same qgz file from subtask 3.1. The hillshade model was already uploaded on the file.
-  The raster hillshade tool was used to create a hillshade model on the dtm layer.
-  This layer was then exported as tif file 'hillshade_model'
 
### Detailed steps taken: 
Loaded the qgz file from Task 3.1 on qgis>> Used the hillshade processing tool under “Raster terrain analysis”>> selected the”burial mounds” dtm layer provided in the file>> Processed the dtm layer”Run”>>Saved the hillshade model as a tif file in Task3/Data/Derived.
 
Comparison of the two DTM layer (hillshade model) and the burial mounds from the georeferenced picture:
The general area of both the maps are correct but they are strictly not superimposable. The distance between the mounds was measured using the measure scale, for example the distance between point 3(on the DTM layer) and the mound on the georeferenced layer is 5.8m and point 18 and the layer is 8m. And some mound points there on the georeferenced layer are missing on the dtm layer and vice versa.  


## Sub-Task 3.3:Elevation of mound relative to direct enviornment/neighbourhood

### Workflow 3.2 
- On the same file from subtask 3.2.
- A multipoint shapefile was created for both ground and mound points 


The elevation was calculated using two ways.
1. 	By using the point sampling tool. Points were made on the mound and the ground and then their raster values were sampled using the tool. After which the layers were joined using the id and the elevation was calculated by subtracting the ground values from the mound values. The final result was saved as a csv file under mound elevation.
2. 	By using the profile tool. The points were mapped on the dtm layer and the elevation profile was created on a graph as the mouse kept sampling points from mound to neighbouring ground. The peaks indicate the mound while the corresponding dip is the height of the ground.
 
### Detailed steps taken: 
A.	Loaded the qgz file from task 3.2>> Created a new multipoint vector layer and selected 18 points on the mound and id-ed them>>Created another multipoint vector layer and selected 18 point on the ground nearing to the mound points and id-ed them>> Used the point sampling tool and sampled both- saved the first layer as mound height and the second layer as ground height>>Joined the two layers using the id as join field and target field>>Opened the attribute table and the field calculator and subtracted the ground height from the mound height to get a new field of values ”mound_elevation”>> saved the layer as a geopackage as well as a csv file “mound_elevation.csv” in Task 3/Data/Original/Elevation Profile.

(Alternatively the elevation was measured a second way as written below) 
B.	Loaded the file from task 3.2>> Used the terrain profile tool>>Added the DTM layer in “Add layer”>>Ran the mouse  between points on the mound and nearest ground point(the shape file for this is saved as “elevation_profile” as a geopackage)>>Produced a graph showing the height difference and therefore the change in elevation>>saved the graph as a png in Task 3/Data/original/images as “Elevation profile”

## Sub-Task 3.4: Digitization of rectangular structures in ENE direction. 

### Workflow 3.4 
- On the same file from subtask 3.2.
- The East north east point on the map was first identified.
- A polygon was skected on a temporary scratch layer.
- This layer was then exported as a geopackage.

### Detailed steps taken: 
Loaded the file from task 3.3>>Changed the shade of the hillshade to red to better view the rectangular structures>>Created a polygon temporary scratch layer>> sketched around the desired rectangle shape>>Exported the layer as a geopackage in Data/Original/Polygon digitized. Created a second polygon temporary scratch layer>> sketched around the rectangle shape>> Exported the layer as a geopackage under the same layer “Rectangle”.

