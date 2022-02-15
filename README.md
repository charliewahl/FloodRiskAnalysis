# FloodRiskAnalysis
Rivers are the lifeline of our cities but occasionally, they can become threads to buildings, infrastructure and human lives. Flood events can show up annually, but bigger and unexpected events hit the local community hard and bring about huge damage. Authorities need an overview, what areas could be affected if weather conditions like heavy rain are recognized. An important tool to take measures in advance are flood risk maps.

# Definition

Risk = probability of occurrence * expectation of damage The goal is not to show regions most likely to be flooded, but visualize areas are most likely to experience damage to infrastructure, buildings and citizen by floods.

# Required data

1) DEM with resolution of 1x1m of the city Riesa
Go to the website for geodata of Saxony and download the tiles 3805684 and 3825684 as DGM1, like in the screenshot below
https://www.geodaten.sachsen.de/downloadbereich-digitale-hoehenmodelle-4851.html

![name-of-you-image](https://github.com/charliewahl/FloodRiskAnalysis/blob/main/Download_DEM.png?raw=true)


2) Vectorlayer for buildings: https://www.geodaten.sachsen.de/downloadbereich-hausumringe-4174.html
3) Vectrolayer for rivers and streets (Download Basis-DLM Shape): https://www.geodaten.sachsen.de/downloadbereich-basis-dlm-4168.html



# Required software
   QGIS
   SAGA
   Gdal

# Methodology
1) Preparing layers

Copy your DEMs and rename the copies as “dem_riesa1.tif” and “dem_riesa2.tif” 
Use the OSGeo Shell to merge the two DEM tiles to one tif with following command
Gdal_merge -o dem_riesa.tif dem_riesa1 dem_riesa2
Load your new tif into QGIS

2) Flood occurrence

Fill Sinks:
Open the toolbox in QGIS and search for “Fill sinks (Wang & Lui)”. If SAGA is not installed on your QGIS, go to plugins and install the plugin SAGA. For the inputlayer, choose the dem_riesa. Make sure that for the output layers, the tick is only at “Filled dem”. You will not need “Flow Directions” and “Watershed Basins” and by not creating them, the program will not need that much time. After running the program, there will be a new layer, that looks similar to the DEM. Export it as a GEOTiff file with the name “filled_dem”.

Alarm points:
The city of Riesa published four different water levels, which are alarm points for the local community, because of flood events with different intensity. You will create four layers, which are showing the situation of the four water levels.
This step you will need to do four times to create four flood scenarios. Open the raster calculator in QGIS and time in following commands:
-	( ( “filled_dem” <= 94.8 ) = 1 ) AND ( ( “filled_dem” >= 94.8 ) = 0 )
-	( ( “filled_dem” <= 96 ) = 1 ) AND ( ( “filled_dem” >= 96 ) = 0 )
-	( ( “filled_dem” <= 96.8 ) = 1 ) AND ( ( “filled_dem” >= 96.8 ) = 0 )
-	( ( “filled_dem” <= 97.6 ) = 1 ) AND ( ( “filled_dem” >= 97.6 ) = 0 )

Safe the outputlayers at your local machine and name them “alarmpoint_1”, “alarmpoint_2”, “alarmpoint_3” and “alarmpoint_4”.

