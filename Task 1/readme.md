## Task 1.0 (`Adiel Batson`)

### Sub-Task 1.1: Plot Mean Annual Temperature vs Altitude
The annual mean temperatures of years 2017, 2018, and 2019 versus altitude for the DWD stations in Bavaria were plotted in one diagram (xy scatter plot). The altitudes from the station description file `KL_Jahreswerte_Beschreibung_Stationen.txt` for the data set ../annual/kl/historical/ were also used.

##### Workflow 1.1.1
- clone repository `EE_3.07_Geodata_WS2021` from the provided repository `https://github.com/rolfbecker/EE_3.07_Geodata_WS2021`
- create environment `geop` in anaconda prompt with directory in folder `gdms0180_DWD_NRW_Annual_Temp_vs_Altitude`.
- download packages; pandas, numpy and update python version.
- download filezilla.
- complete tasks in given template `gdms0180_DWD_NRW_Annual_Temp_vs_Altitude` with annonations to code.
 
The file `ts_appended.csv` is created in the notebook `gdms0180_DWD_NRW_Annual_Temp_vs_Altitude` containing all relevant data for Bayern. This dataset however, needs to be edited with `pandas`to fix the columns, rename them and sort only the necessary data; 
- Date, Station name, Station number, Station altidue, Mean Annual Temperature. 

##### Workflow 1.1.2
- import `ts_appended.csv` to a new notebook called `Dataframe Fixing & Graphs Plotting.ipynb`
- operations to rearange the dataframe are annotated. `df_6` is produced and saved as a csv file; `df_6.csv`
- the graph of the mean anual temperatures of years 2017, 2018 and 2019 VS the altitude of DWD stations was plotted and saved as `bayern_station_altitude_vs_annual_mean_temperature_2k17-19plot.png` using `df_6` parameters.



### Sub-Task 1.2: Create a Map
Due to technical issues 2 DTMs of Bayern were used:
1. DTM50: provided from geodaten.bayern.de: 50m horizontal resolution in EPSG:25852 as GeoTiff (500 MB):
http://www.geodaten.bayern.de/opendata/DGM50_UTM32/dgm50_epsg25832.tif
2. DTMsonny: sourced online: DTM Germany 50m v1 by Sonny: https://data.europa.eu/data/datasets/dtm-germany/?locale=de (Data.europe.eu, 2022).

 Important: DTMsonny has altitudes of exact values to that of the stations.  

Using these DTMs in tandem with digital administration boundaries sourced from the Bayerische Staatsregierung (2022) a map was created (including title, annotations, scales, legends, north arrow, graticule, CRS, data source references, etc.) with all DWD temperature stations in Bavaria which were active in the years of concern (2017, 2018, 2019). 

##### Workflow 1.2
-This was done by first retreiving the opendata file from opendata.dwd.de using the FTTP protocol over jupyter-lab. The file was then modified using pandas, afterwhich a csv was created with the desired stations (in Bayern) for the desired time range and saved as `ts_appended.csv`. This csv file was imported to QGIS in CRS EPSG:25832 - ETRS89 / UTM zone 32N. This is the coordinate system of the project.
- Bayern was added into QGIS from Bayerische Staatsregierung (2022) as a vector layer of the state of bayern and a vector layer of the administrative borders in the state (shape files located in Git folder `All Data Files`).
- The file `ts_appended.csv` was added to QGIS and the stations were marked, annotated by their station numbers and recolored. They symbols were also selected.
-  Both DTMs were cropped precisely to the Bayern border vector layer `Bayern State Border` and color corrected as follows to a new layer called `Altitude And Color Correspondence`:
    - green = low lands   <700m altitude
    - brown = mountainous areas   >700m> 1400m altitude
    - white = The Alpine peaks   >1400m altitude

Important: The color ranges are exaggerated to represent these 3 primary domains; eg. when not exaggerated the color ranges for the alpine peaks would be more difficult to notice.

- The DTM50 was then duplicated to create a contour layer at 250m intervals (closer intervals created to many linear demarkations limiting the clarity of the map) and a hillshade texture which was colored using the one channel pseudo color tool to create a color range. These were orginally generated using the QGIS tool box to run the respective functions, but after several crashes I decided to style them based on the attributes in the symbolism tab of the proterties task bar. 

- The layer `Altitude And Color Correspondence` was later made 81.1% transparent so that the hill shade texture could be visible through the contours, station demarkations and generated colors of the terrain. 
- The QGIS tool `fill sinks (Wang & Lui)` was then used to prepare the terrain for hydrology data extraction from DTM50 & DTMsonny.
-The tool `strahler order` was then used to extract hydrogeological features in the DTMsonny as an error repeatedly occured with DTM50. 
- catchment areas in Bayern were then produced and saved to the layer `Water Catchment Areas` 
- Corrupted data sets of river channel networks, flow directions and other features were also produced but not formattable enough to justify keeping them as layers in the  projects. These were later deleted.

###### Important: These layers are not uploaded to the repository because they are large .tif files. This method describes how they were produced; Primary input DTMs are `DTM50` and `DTMsonny`!
Layers at the end of the project should follow in this order, so that map is correctly recreated:
- Bayern state border
- district/administrative borders
- Active stations 2017-2019 by station ID (`ts_appended`)
- DTM contours: 250m intervals  (duplicate .tif file)
- Altitude & Color correspndence (m)   (duplicate .tif file)
- Hill Shading Textures    (duplicate .tif file)
- Water catchment Areas (can be toggled on/off to show layer)  (generated .tif file)
- DTMsonny x DTM50 sampled_station_values (sampled altitues for subtask 1.3) | (attibute table and csv table: `DTM_50_sampled_werte.csv`)
 

### Sub-Task 1.3: Sample the DTM at the locations of the DWD stations.
##### Workflow 1.3
- The QGIS Processing Toolbox was utilized. The dialog "Sample Raster Values" from the "Raster Analysis Toolbox". Selecting the menu item Processing -> Toolbox toggles the visibility of the Toolbox pane. From this Raster Analysis was selected. 

- Another field  was added to the DWD stations attribute table with the altitudes sampled from both DTMs. 

- An attibute table was then exported with the sample heights of DTM50 and DTMsonny in the file `DTM_50_sampled_werte.csv`.
`Important`: in the layer `Sampled_stationaltitudes_DTMsonny x DTM50` both altitudes of both DTMs are sampled in the attribute table.
- This csv file `DTM_50_sampled_werte.csv` was then exported to the notebook `Dataframe Fixing & Graphs Plotting.ipynb` where its values were concatonated to help form the dataframework `df_6`.

###### Q & A
`Compare the original altitudes from the DWD station file to the heights derived from the DTM. Where and why are the strongest deviations?` 
As a means of comparison of the DTM altitudes vs the station altitudes a linear regression was performed to see where the strongest devations are. This graph can be found in the folder `1.1 -1.4 Graph Images` and is called `Linear_Regressions_DTM_Altitude_vs_Staion_Altitude.png`. The why aspect of the question was inconclusive, I've made several inferences;
 - Normal-Null (NN) is an outdated method of measuring altitude, which the station file uses station file uses (Bundesverband Geothermie, 2020). The difference in calibration of null values could result in altitudinal differences. The fact that the DTM heights more closely match the real life station heights compared to the station file supports this.
 - The DTM uses LIDAR imaging to map altitudes, while the station ID uses different methods; perhaps the difference is partly here to blame.
 - The effect of pressure on alititude readings could also play a role on altitude meters which use mecury to measure. 


### Sub-Task 1.4: Plot the mean annual temperatures versus the DTM heights for the DWD stations in Bavaria.
A linear regression with numpy was performed to the data (not Excel!!!). How the temperature varies with height according to the regression model was documented in the notebook `Linear_Regressions.ipynb`, i.e. what is the temperature gradient (the slope of the regression line) in units K/m or °C/m?

##### Workflow 1.4
- using the anaconda prompt shell in the environment `geop` the package sklearn was downloaded using `conda install -c intel scikit-learn.` 
- afterwhich, the code was prepared and annotated in the notebook `Linear_Regressions.ipynb`.
- Using the generated file `df_6.csv` a graph was plotted to show the mean annual temperatures versus the DTM altitudes for the DWD stations in Bayern. 
- After attempting to construct the linear regression, it was noticed that df_6.scv contained blank/NaN values in the year columns recording mean annual termperature. Blank values were then programmed to be filled with a mean value from the columns contents. The produced dataframe for the linear regression was then `df_2.` (saved and used as `df_2.csv)`.
- The code is annotated to clarify the exact process to the results in the notebook `Linear_Regressions.ipynb`.

## References

- Bayerische Staatsregierung, (2022), homepage. Date accessed: 04.03.2022: https://geoportal.bayern.de/geoportalbayern/seiten/anwendungen
- Bundesverband Geothermie, (2020), Normalnull. Date accessed: 31.03.2022: https://www.geothermie.de/bibliothek/lexikon-der-geothermie/n/normalnull.html 
- Data.europe.eu, (2022), Digitale LiDAR-Geländemodelle von Deutschland | Digital LiDAR-Terrain Models of Germany. Date accessed: 15.03.2022: https://data.europa.eu/data/datasets/dtm-germany/?locale=de
