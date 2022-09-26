# TASK 4: Creation and 3D visualization of a DTM layer

## Subtask 4.1: Creation of DTM layer (Kshama Gauri) 

### Workflow 4.1 

- On a new project file on qgis, using WMS a NW topographic map was loaded( the DTK Farbe)
- On google earth the desired location was searched and saved as a kml file. 
- This kml file was loaded as a vector layer on qgis and the location was zoomed in on.
- The file NRW_DTM_ESPG_25328_BB_Tiles was loaded as a raster layer from https://github.com/rolfbecker/EE_3.07_Geodata_WS2021/tree/main/data/derived/NRW_DTM_NRW_EPSG_25832_Tiles_BB
- This layer was selected and using the tiles selector, the desired tiles were selected. 
- The selected tiles were loaded on the qgis tile downloader plug-in uploaded by [Khaymanr](https://github.com/khaymanr/qgis_tile_downloader)
- The tiles were  processed and produced in the folder Task 4/Data/Original/NRW_DTM_SAUERLAND_tiles as XYZ zip files. 
- The tif file was also produced using the simultaneously in the same folder. 
- The tif files were loaded as a raster layer on the qgis project. 
- The files were merged using the GDAL(Raster) merge tool
- The DTM layer was  exported as tif file.


## Subtask 4.2: 3D Visualization of the DTM (Daphne Larose & Adiel Batson)

### Workflow 4.2 
- Having produced the tif file, it was imported as a raster layer to a new QGIS project.
- This tif file was duplicated into 2 DTMs;
   - Using the raster terrain analysis a hillshade model was produced on one DTM and saved as a permenant layer. 
- The other duplciate was retitled `Sauerland_DTM` and a color gradient was developed staring with greens for lowlands and browns for more mountainous/higher altitude regions in Sauerland. 
- To switch to a 3D view, `New 3D View` is selected from `View` menu. 
- In the 3D configuration, the `DEM (Raster layer)` is chosen with a vertical scale of 2.00 to emphasise the relief and tile resolution of 64px .
- Using the [script](https://raw.githubusercontent.com/klakar/QGIS_resources/master/collections/Geosupportsystem/python/qgis_basemaps.py) by Klas Karlsson in the `python console` feature, a series of XYZ tiles  were imported into the QGIS browser and used. The XYZ tiles, often from satellites, are used as a texture layer on top of the DTM. 
- From the selection the Bing Visual Earth is chosen. 
- A shapefile for the waterways of the region of the DTM was used. 
- A new temporary scratch layer was created to match the dimensions of the DTM.
- With the use of the processing tool and vector geoprocessing, the option clip vector by mask layer created a new clipped shapefile for the waterways for our DTM.
- The 3 layers on top of each other give us a proper 3D visualisation with relief. 
- the 3D map is exported as a series of images and then converted to a gif and a .mov file.

## 2D and 3D Views 
![](https://media.giphy.com/media/GmBnKWavRNIHTknFsn/giphy.gif)

## Final Result 
![](https://media.giphy.com/media/DrvVO6X0skWzP5nb19/giphy-downsized-large.gif)


### References
1. '3D DEM Visualization In QGIS 3.0' (2018)https://opengislab.com/blog/2018/3/20/3d-dem-visualization-in-qgis-30
2. Geofabrik GmbH &  OpenStreetMap Contributors (2018) http://download.geofabrik.de/europe/germany/nordrhein-westfalen.html
3. Karlsson, K., https://raw.githubusercontent.com/klakar/QGIS_resources/master/collections/Geosupportsystem/python/qgis_basemaps.py
4. Khaymanr & Lara, Harley https://github.com/khaymanr/qgis_tile_downloader

