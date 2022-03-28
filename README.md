## Flood Risk Analysis

Rivers are the lifeline of our cities but occasionally, they can become threats to buildings, infrastructure and most importantly human lives. Flood events can show up annually, but bigger and unexpected events hit the local community hard and bring huge damage. Authorities need an overview of the affected if weather conditions like heavy rain is recognized. Important tools for takeing measures in advance, are flood risk maps.

# Definition

                              Risk = probability of occurrence x expectation of damage
   * It is our goal not to show regions most likely to be flooded, but visualize areas that are most likely to experience damage by floodings such as Urban Areas, Infrastructure, Argricultural Land or Green Areas.

# Required data

   * Copernicus Landuse Data in Riesa: Urban Atlas LCLU 2018 provides reliable high resolution land use and land cover Data in vector format for the              reference year 2018 (https://land.copernicus.eu/local/urban-atlas/urban-atlas-2018). ! In order to download Data from the Website you have to create an account beforehand. !
   * Vector Data for visualizing areas with different risks during flood events in Riesa: OpenStreetMap Data Extracts from GeoFabrik                              (https://download.geofabrik.de/europe/germany/sachsen.html)
   * High resolution Raster Data for Riesa: Digitiales Geländemodell (DGM1) with a grid ize of 2 km x 2 km and a resolution of 1 m x 1 m, !free to download!
     (https://www.geodaten.sachsen.de/downloadbereich-digitale-hoehenmodelle-4851.html)
     
 # How to download the required Data   
 
 <details>
   <summary><b>File storage</b></summary>
<br/>
  
  * extract and have a look around the given 'FloodRiskProject.zip' folder.
  
    * inside you will find a folder called 'Data', in which the required Vector- and Rasterdata will be stored in. Besides, we built two QGIS-Models  `Cartography&Styles.model3` and `FloodRiskMap.model3`. 

    * `RasterData` and `Vectordata` folder consist of `.bat`- files which give further instructions on how to process the required Data for the models. 

    * Aditionally, inside the Vector- and Raster Data Folder we stored preset Vectorstyle-layers in `.png`-Format for further usages that will be shown further down. 
 > Note: It is important that QGIS-Styles in `.png` are stored in the same folder as the Layers loaded into QGIS. Otherwise, if this would not be the case, the loaded Layers would be shown in a random default style, even if the Styles are added into the QGIS-Model  
</details>
 
<details>
   <summary><b>How to aquire Raster Data</b></summary>
<br/>
   
  * **Digitales Geländemodell (DGM1)** <a href="//https://www.geodaten.sachsen.de/downloadbereich-digitale-hoehenmodelle-4851.html">Sachsen.de/downloadbereich-digitale_hoehenmodelle</a> and download tiles 3805684 and 3825684 as shown in the screenshot below
 
 ![name-of-you-image](https://github.com/charliewahl/FloodRiskAnalysis/blob/main/Download_DEM.png?raw=true)
   
   * download  and extract the DGM1 tiles into `Raster Data` Folder. In each of the two ectracted folders  you will find a `.xyz` tile which will be used in further processing and a Spreadsheet with specific information on your downloaded tiles. For the sake of simplicity, copy both of the downloaded `.xyz` Raster tiles into the `Raster Data` folder.
   
 </details>      
   
 <details>
   <summary><b>How to aquire Vector Data</b></summary>
<br/>
 
* **URBAN ATLAS 2018** Landuse <a href="https://land.copernicus.eu/local/urban-atlas/urban-atlas-2018?tab=download">Urban Atlas 2018</a> and select `Dresden` in the `Download`. After selecting the Data ýou need to download and extract the files into the `Vector Data` Folder. After ectracting the Data you should have a Geopackage called `DE009L2_DRESDEN_UA2018_v013.gpkg`
   
* **Geofabrik OSM Data** visit <a href="https://download.geofabrik.de/europe/germany/sachsen.html">Geofabrik Sachsen</a>. Download and store the OSM Data into the `Vector Data` Folder in Shapefile-format (`sachsen-latest-free.shp.zip`) inside the `VectorData`-folder. In addtition it will be necessary to ectract the Data to get access
   
   </details> 
   

# Required software
   * QGIS Desktop 3.16 including the Plugins

     * SAGA 7.8.2-14 
     * GDAL 3.4.2

   * Visual Studio Code 1.65.2 (or any other texteditor application to open `.bat` files)
   
  

# Methology

<details>
   <summary><b>1. Pre-processing</b></summary>
<br/>
   
1. Before using our downloaded Data in `QGIS` it has to be preprocessed via `Gdal`. Therefore we have created `.bat` inside the `vector Data` as well as in the `Raster Data` folder,containing a short documentation on how to process the Data. 
  
2. After running through preprocessing you will be able to use the files as inputs for the follwoing Models and run the Analysis automaticially.
   
 </details>      
   
 <details>
   <summary><b>2. setting up and running the QGIS Models</b></summary>
<br/>   

   1. Open the QGIS application and open the `FloodRiskMap.model3` via the Data Soucre Manager.

   2. Add the required data inputs into the Model 

      1. **extent of merged DGM1**: this input is necessary to get the `gdal_calc.py` running properly an of course for defining a Region of Interest  we need to specify the extent of our Project. For simplicity reasons we used the extent of the merged DGM1 `riesa_merged`, which will atomaticially be calculated after setting the Rasterlayer as an input here.

      2. In **Landuse reprojected** we will need the unpacked Vector Layer `DE009L2_DRESDEN_UA2018.shp` from the `Landuse Shapefiles` folder from the steps before. While running the Model, the Landuse Layer will be split into categories and  eventually given weights according to the vunerability of the chosen categories (for more details have look at `documentation`or **`FloodRiskModel.png`).

      3. **DGM1 merged + reprojected** is the preprocessed Raster Layer `Riesa_merged` which on the one hand side will be used for creating an Area of Interest for our project and on the other hand side it will work as the fundamental input for our Flood Risk Secenarios by `Filling Sinks (Wang & Liu)` and using the `Gdal_calc`.

      4. The following Model outputs `weighted categories` and `alarmpoints_merged` have only been added for demonstration reasons-it is up to you whether you want to have a look at them or not
   
      5. The output `Flood Risk Map` is the final output. This means the box will be left as ticked.
   
      6. `water` is a Category output from the `Landuse reprojected` layer, which will be needed for the final map styling. Save this file under the name `landuse_water´ in the same direcotry as the OpenStreetMap layers e.g. `sachsen-latest-free.shp` 
      
      7.  Finally run the Model and have a close look at your outputs!
  
  3. Open the Model `Cartography&Styles.model3` as done with the previous Model.
  
      1. All the needed Layer Styles  to be copied from the folder `` into the same folder as the downloaded OpenStreetMap Data from <a href="https://download.geofabrik.de/europe/germany/sachsen.html">Geofabrik Sachsen</a> (Otherwise the Styles won't be applied ot the layers in QGIS). 

       2. Remove all the layers which don't match with the style names afterwards (because we won't need them).
 
       3. the specific stylings of the OpeStreetMap geometries are stored under the same name as the geometries exept that they come in `.qml` -Format. Therefore it is necessary to browse the stylings of each layer from the `sachsen-latest-free.shp` into the Model.
  
       4. As before the **DGM1 merged + reprojected** `Riesa_merged` will work as our Area Of Interest for clipping the Layers by an extent.
  
       5. Additionally you will have to browse the initially preprocessed OSM files from your files and set them as individual inputs. If you ran through the files as stated in the preprocessing the input files should have the same name as written above the boxes in te modelbuilder.
  
       6. Run the Model
  
       7. Set the order of the created Layers as stated in `QGIS_layer_order.png`
          
   


         
 
   
   


