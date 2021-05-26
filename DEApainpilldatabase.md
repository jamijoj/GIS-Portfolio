# Using ArcGIS Insights to investigate the DEA's pain pill database


<img width="1000" alt="Mapscreenshot" src="https://user-images.githubusercontent.com/73584997/113980331-55222100-9814-11eb-8992-340b2ecbe98a.png">

<iframe src="https://insights.arcgis.com/#/embed/5f0bb48ee02a4e6492412d7fb5581147" width="1360" height="590" frameborder="0"></iframe>

This map is the result of an exericse we did for class using ArcGIS Insights, Esri's answer to Tableau. The project asked us to examine the Drug Enforcement Administration's pain pill database, which contains information on all pain pills sold in the US, including who sells them and where the pills go. [This Washington Post article](https://www.washingtonpost.com/graphics/2019/investigations/dea-pain-pill-database/) has a section to easily sort the database by county and retrieve a corresponding dataset. I sorted for Mingo County in West Virginia and downloaded the dataset, which I then imported into a new workbook on ArcGIS Insights Desktop.
I also imported the county boundary shapefile for Mingo County and added it to the map.  
&nbsp;  
I turned this into a link map by creating address layers from the address information for buyers and manufacturers in the dataset. After I added them to the map, Insights automatically created links to show the network between buyers and manufacturers. I used graduated symbology to visualize the buyers and manufacturers by number of prescriptions filled. I also visualized the links lines whose thickness increased based on sum of total dosages sent along the path. I chose to use a dotted line to better show the flurry of activity from McKeeson Corporation and buyers in Mingo County. 
The result is a map that is visually appealing that can be explored by users to find out more about how pain pills move in and out of the county. 
&nbsp;  

[Back to GIS Portfolio](README.md)
