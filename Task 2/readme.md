## Task 2 DWD Precipitation Measurements in North Rhine-Westphalia (NRW) (Daphne Larose)
### Spatio-Temporal Precipitation Anmination using PostGIS together with QGIS Temporal Controller

Produce a rainfall video over a 7-day period in 2021 that covers the heavy rains that led to the catastrophic flooding. The animation should cover all DWD stations in NRW with hourly precipitation. Create a point layer in QGIS showing the stations in NRW with time dependent precipitation rate data (mm per hour) in the attribute table. This layer has to come from a PostGIS view, which joins the two tables with 1) static station info including coordinates and 2) time dependent precipitation rates. Use the QGIS Temporal Controller to produce an animation.

### Open Source Tools
1. PostgreSQL
2. PostGIS
3. QGIS Temporal controller 

### Sub-Task 2.1: Creating and filling the geodatabase
Create and fill your own geodatabase with DWD precipitation station data as well as hourly precipitation time series. Follow the Jupyter Notebook tutorial of `geo0930_PostGIS_Insert_DWD_Stations_and_TS`

##### Workflow 2.1.1: Creating the geodatabase
- The role `geo_master` was created by first connecting as user postgres to the maintenance database postgres and using the command line `psql -U postgres -d postgres -f 010_create_database_geo_create_role_001.sql` in the Anaconda Prompt, in an environment that was created called `geo` and not in the base environment, along with the SQL script `010_create_database_geo_create_role_001.sql`. 
- Using the same SQL script containing the sql command that enables the user postgres to connect the `geo` database, the schema `dwd` was created with `geo_master` as the owner. 
- The extension postgis was created using the command line `psql -U postgres -d geo -f 012_create_extension_postgis_001.sql` and the SQL script `012_create_extension_postgis_001.sql`.

##### Workflow 2.1.2: Adding time series PostgreSQL/PostGIS

- The Notebook `Task2.ipynb` was created based on the notebook `geo0930_PostGIS_Insert_DWD_Stations_and_TS` from  the [opengeo](https://github.com/rolfbecker/opengeo) repository and was used to reformat and merge time series for hourly precipitation from the Climate Data Center (DWD). 
- The specific parameters such as the state (North Rhine-Westphalia) and stations that were operational in 2021 are filtered to provide the necessary data from the open source DWD data [here](https://opendata.dwd.de/climate_environment/CDC/) .
- First connect to the FTP directory and then create a pandas dataframe from the FTP Directory listing.
- The files are downloaded by using the GrabFile function in python.
- Then the columns are renamed appropriately and the stations for NRW that were operational in the 7 day period we need are chosen.
- A geopandas dataframe is then created, which is the same pandas dataframe but with an extra column called `Geometry` which is an object of type point, from the longitude and latitude.
- The next step is to connect to the PostGIS and set the connection parameters as User: `geo_master`, password: `xxxxxx`, host: `localhost`, Port: `5432`, database: `geo` and a connection URL is obtained.
- By using the `sqlalchemy` package an engine database is created to pass information from the notebook directly to PostGIS in our schema `dwd`. 
- The zip archives for the time series is then downloaded and a new pandas dataframe with the hourly precipitation, the precipitation values and the station_id is created and is also passed on to PostGIS.
- These two dataframes can then be seen as tables and are accessed in **pgAdmin 4**.


### Sub-Task 2.2: Add the PostGIS view v_stations_prec as a layer to your QGIS project
 After having finalized the tutorial above the geodatabase geo contains the two tables dwd.prec and dwd.stations as well as the view v_stations_prec. The view joins the two tables. Add the geo-enabled view as a PostGIS layer to your project.

##### Workflow 2.2
- The tables `prec` and `stations` are merged and the hourly precipitation for a 7 day period (from the 11.07.2021 to 17.07.2021) is chosen.
- A new view **dwd.v_stations_precipitation** is created in the schema `dwd` using the joined tables and the command line `create view dwd.v_stations_precipitation as (select t1.station_id, t2.ts, t2.val, t1.geometry from dwd.stations t1, dwd.prec t2 where t2.ts between '2021-07-11T00:00:00UTC' and '2021-07-17T00:00:00UTC'and t1.station_id = t2.station_id)` in the query tool.
- Since the view was originally created as the postgres user, the user `geo_master` was granted select. 
- The was then created by the user `geo_master` in the `geo` database.
- The view is first dropped and then while being connected as the user `geo_master` the view was created using the SQL script `014_create_view_v_stations_precipitation_v001.sql`.
- This then made the user `geo_master` the owner of the view.
- The view **dwd.v_stations_precipitation** and the stations are then added to QGIS as PostGIS layers.
- To do so we first have to add the geo database in QGIS by going in View >> Add PostGIs layers.
- A new connection is created, with the Name: `geo`, Host: `localhost`, Port: `5432`, SSL Mode: `Prefer` and then in Authentication >> Basic and put the username :`geo_master`. 
- From there it is now possible to connect to the geo database with the password : `xxxxxx`as set in the Notebook `Task2.ipynb` and then open the dwd schema.
- To see and select the view, `Also list tables with no geometry` has to be chosen and the `station_id` and `ts` have to be selected as Feature ID and then we can add the view `v_stations_precipitation` and the stations to our project.
- The NRW administrative boundary (vector layer) as a Web Feature Service (WFS) and the topographic map from the NRW WMS are added as the background.

### Sub-Task 2.3: Use the QGIS Temporal Controller to produce an animation
Follow the tutorial https://www.qgistutorials.com/en/docs/3/animating_time_series.html on QGIS Temporal Controller to produce an animation.
The time period of data and animation, respectively, has to cover 7 days with hourly resolution. You have to change the creation of the view in the Jupyter Notebook to extend the time span. The catastrophic rain event of 2021 (less than two days) should be roughly in the middle of the view's selected time interval.
Import the NRW administrative boundary (vector layer) as a Web Feature Service (WFS). We discussed in class how to import and use the web services (WMS, WFS, WCS) provided by NRW (as part of opengeodata.nrw.de). Import the topographic map from the NRW WMS collection as the background map.
Use the menu view -> decorations to add title, legend, scale, north arrow, time stamp, etc.

##### Workflow 2.3
Following the [tutorial](https://www.qgistutorials.com/en/docs/3/animating_time_series.html) by Ujaval Gandhi :
- The symbols for the layer `v_stations_precipitation` are changed to a heatmap and the most suitable colour ramp was chosen along with the desired radius size and opacity, in this case the colour ramp Turbo was used. The column `val` is selected in `Weight points by`.
- In the properties of the layer `v_stations_precipitation` the Dynamic Temporal Controller was turned on, `Single Field with Data/Time` was chosen and the time series `ts` set as the field in the Configuration . 
- Using the Animated Temporal Navigation from the Temporal Control Panel in the toobar, the animation was activated. 
- The title,time stamp, scale, north arrow and legend were then added from `Decoration` in `View`.
- The animation was then exported as series of images, which then had to be converted to a `gif` and `.mov` with the website https://ezgif.com/maker.


#### Hourly precipitation in North Rhine-Westfalia
![](https://media.giphy.com/media/1Gv0dSSVslVMu9ChXD/giphy.gif)

### References
- Becker, R., '#eolab_de - geo0930: Geopandas, PostGIS and QGIS TimeManager' , https://www.youtube.com/watch?v=wvIkhZNfz6s
- Gandhi, U. , 'Animating Time Series Data (QGIS3)', https://www.qgistutorials.com/en/docs/3/animating_time_series.html

