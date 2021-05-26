# Project: Mapping the British Museum

![FinalMap3](https://user-images.githubusercontent.com/73584997/119645151-b796a700-bdeb-11eb-9438-0d009dc1e202.png)

## Background
<details>
<summary>Read more</summary> 
<br>
The British Museum has long been a source of controversy. It is an encyclopedic museum whose mission is “to hold for the benefit and education of humanity a collection representative of world cultures ('the collection'), and ensure that the collection is housed in safety, conserved, curated, researched and exhibited” (britishmuseum.org). However, many of the objects in the collection are either suspected or confirmed to have been looted from countries that imperial Britain colonized. Well known examples are the Parthenon Marbles, the Rosetta Stone, and the Benin Bronzes, though there are many other objects in the museum’s eight million object collection that are of dubious provenance. The purpose of this project was to create a tool to act as a visual aid  to shed light on the nature of the museum’s collection. The tool includes interactive elements and quantified visuals that communicate the magnitude of the issue to the public. Additionally, with more information on looted objects, these deliverables can be developed further to build a predictive model to assess any object at the British Museum for likelihood that it was looted. Finally, this may be used as a model on how to approach decolonizing other museum collections.
</details>
  
## Approach, Methodology, and Analysis
<details>
<summary>Step 1: Conduct background research and create work plan</summary> 
<br>
For this first task, I researched the history of the British museum, how to download British museum collections information into a dataset, and I located information that could be useful for creating a geospatial data of British invasions and occupations
</details>

<details>
<summary>Step 2: Create a dataset of the British Museum collection</summary> 
<br>
The British Museum has made their collections database publicly available at https://www.britishmuseum.org/collection. The database has been in progress for 40 years and is still not done – there are over 8 million pieces in the museum, and only around half of those have been uploaded to the database so far. Access to only half of the objects in the collection is not a problem for this project since 4 million objects is still a good sized sample, but future work could include updating this project to use all of the items in the collection, once the database is finished.

This task took considerably longer than anticipated, in part because the British Museum took down the public SPARQL endpoint that was used to query their collections database. I was left to manually download the necessary datasets through their online collections website. The biggest frustration with this was that downloads of more than 10,000 pieces are not allowed. I didn’t have time to build a web scraper for this purpose, so I went to the collection online, used the filter feature to filter by region (Africa, Oceania, the America, Asia, Europe) and then, for each region, filtered by preset subject filters for arts/architecture, ceremony/ritual, society/human life and religion/belief. At the end I had over 100 datasets downloaded, which I combined into one dataset using Python. I next cleaned the dataset in Python using the following steps:

 1. Removed rows with duplicate values
 2. Removed objects that were photographs by removing rows with “Technique” listed as any of the following: Albumen printing, gelatin silver printing, photograph, glyphograph , negative, photograph, postcard. This removed over 100,000 rows.
 3. Removed objects listed as coins, which removed 333,000 rows.
 4. Filled NaN values with 0
 5. Removed unneeded columns (“Image,” “Denomination,” “Escapement,” “Type series,” “Dimensions,” “Inscription,” “Curators comments,” “Bib references,” “BM/Big number,” “Reg number,” “Add ids,” “Cat no,” “Banknote serial number,” “Joined objects,” “Authority,” “Condition,” “School/style,” “State,” “Ethnic Name (made by),” “Ethnic name (assoc),” “Ware,” “Subjects,” “Assoc name,” “Assoc place,” “Assoc events,” “Assoc titles,” “Acq name (excavator),” and “Acq name (previous).” 

The cleaned dataset had approximately 857,445 rows (each corresponding to an object) and 10 columns. The columns were:
 - *Object_type:* Type of object (painting, pottery, etc) 
 - *Title:* Name of object, if exists
 - *Description:* A brief description of the object
 - *Culture:* The culture from which the object came
 - *Production date:* The confirmed or estimated year the object was made
 - *Production place:* The place the object was made. Recorded either as city, country, POI, or a mix of all three
 - *Find spot:* The place the object was “found.” The place the object was made. Recorded either as city, country, POI, or a mix of all three
 - *Acq date:* The year the object was acquired by the British Museum
 - *Acq notes (acq):* Any notes about the acquisition
 - *Location:* Says whether the object is on display and, if so, where in the museum it is
</details>

<details>
<summary>Step 3: Create map in ArcGIS with layer of British occupation by territory and layer of objects in the British visualized by location of origin and date of acquisition</summary> 
  <details>
  <summary>3a.1 Geocode</summary>
The next step in the process was to geocode the objects based in location. The dataset had two location attributes, “Find spot” and “Production place.” The location information has been recorded inconsistently so some objects only have “Find spot,” noted, some only Production place, and some both. To approach this, I decided to geocode by the “Find spot” column first, which is most relevant to the purpose of this project then geocode anything without a “Find spot” by the “Production place” column, since it still reliably approximates the information of interest.

To start, I used Python to concatenate the “Find spot” and “Production place” columns and drop all columns that had a null value in both. That got rid of objects without location information so could not be used for the project. This removed 254,582 objects. The resulting dataset was now 602,863 rows (objects).  

Instead of geocoding 600k + objects, I decided to make a list of all the locations in the dataset, with the understanding that many would repeat as multiple objects correspond to the same location, geocode this list, then join the collections dataset to the geocoded locations. To start, I made a list of unique instances in the “Find spot” column and in the “Production place” column and merged them into a single list while concurrently dropping duplicate values. This gave me a list with 9,087 locations. I knew there were locations that repeated since the way locations were entered is so inconsistent - for example, “Iran,” “Iran, East” and “Iran (archaic)” are all read as separate locations. The 9k entries on this list represent an upward bound of the true number of locations for objects. The process needed to geocode exact locations for each object are beyond the scope of this project; instead, I geocoded by city, when available, and country. 

I used the Geocode Address tool with Esri’s World Map Locator to geocode the “Find spot” and “Production place” columns in the dataset. I geocoded with parameters set to look in all countries, with categories set to  “Populated Places” and “POI.” The results of the first geocode are here:
    <br>
<img width="500" alt="Image1" src="https://user-images.githubusercontent.com/73584997/119650719-237c0e00-bdf2-11eb-9317-3105a47120cb.png">&nbsp;  
*Figure 1: Locations of British Museum objects geocoded*&nbsp;  
<br>
I noticed a high number of objects from the US (highlighted in blue), which seemed odd to me since the British museum is not known for having a large Indigenous American or North American collection. I highlighted these to explore them further and realized that the list of addresses I built from the database contained archaic place names like Naukratis, Pharae, Cleonae, Thebes, Marathon, and others. The geocoder matched these places to cities in the US with the same names. There were also a number of unmatched rows locations that had not been recognized at all. Using the Rematch Address Tool, I rematched all unmatched addresses. Next, I selected all rows by attribute to find objects that had been matched to the US. I manually went through the list and coded locations correctly.
    <br>    
<img width="500" alt="Image2" src="https://user-images.githubusercontent.com/73584997/119650960-7229a800-bdf2-11eb-9ac1-e4211bc3b0d7.png">&nbsp;  
*Figure 2: Mismatched US addresses matched correctly*

The results of this rematch (fig. 2) showed that there were still quite a few points in the US, but many mismatched points belonged in the Mediterranean region, because they were place names in ancient empires like the Roman, Greek, and Byantine empires.

The locator I used was the Esri World Locator, but I had to go through and manually recode addresses that were names of archaic cities. In future, datasets that have a lot of archaic names could be geocoded from a locator created using ancient city names and their modern equivalents. [This is an example](https://pleiades.stoa.org/downloads) of such a dataset. 

Here is the final map with locations geocoded:
    <br>
<img width="500" alt="Image3" src="https://user-images.githubusercontent.com/73584997/119654323-54f6d880-bdf6-11eb-86c5-dc332c44ce78.png">&nbsp;  
*Figure 3: Object locations geocoded*

This is a visual representation of where objects in the British collection come from. It’s not surprising that many objects have origins in different places in Britain. It’s interesting to see that India and the Mediterranean region are also highly represented. In Africa, places along the coastline seem to have a high representation in the collection. At first glance, it seems that most object origins were in Britain and India.  To be sure, I did a hot spot analysis using the Optimized Hot Spot Tool:&nbsp;  
<img width="500" alt="Image4" src="https://user-images.githubusercontent.com/73584997/119654696-ba4ac980-bdf6-11eb-9bfb-bd4b14258d21.png">&nbsp;  
<img width="500" alt="Image5" src="https://user-images.githubusercontent.com/73584997/119654705-bc148d00-bdf6-11eb-84e7-6598d4887f00.png">&nbsp;  
*Figures 4 & 5: Hotspot analysis, zoomed to Britain*&nbsp;  
The hotspot analysis showed that there was a meaningful cluster of locations in and around Britain.
</details>

  <details>
  <summary>3a.2. Plot objects to as points to layer</summary>
The next step was to plot the objects in the collection to the map by joining the rows to their corresponding geocoded locations. I imported the collections CSV into ArcGIS and did some basic exploratory analysis by creating charts from the collections data.
<img width="500" alt="Image6" src="https://user-images.githubusercontent.com/73584997/119656879-39410180-bdf9-11eb-8c9f-6a05e71fc975.png">&nbsp;  
*Figure 6: Objects aggregated by object type*&nbsp;  
The most common  object type is “print.”
<img width="500" alt="Image7" src="https://user-images.githubusercontent.com/73584997/119657065-73120800-bdf9-11eb-9448-287251b1fa63.png">&nbsp;  
*Figure 7: Objects aggregated by date*&nbsp;  
    
There was a peak in acquiring objects around 1818. Collecting picked up in the mid-1800s and remained steady. There are also quite a few number of objects in the collection that do not have the date of acquisition included (57,183 rows with “0” in the acquisition date column).

I then joined the collections data table to the geocoded locations layer. For this, I created a new field called “JoinLoc” that copied data from the “Find spot” column, but if null, would fill with the “Production place” field. I then executed a table join to create the new layer. 

I ran another hotspot analysis to see if the results would be different since each point now corresponded to an object, not just a location, but the results were the same – the majority of objects were of British origin (fig. 8).
<img width="500" alt="Image8" src="https://user-images.githubusercontent.com/73584997/119657452-eb78c900-bdf9-11eb-8b38-d462f05ed246.png">&nbsp;  
*Figure 8: Hotspot analysis 2*&nbsp;  
</details>


 



