
# Project: Lack of Transit Access as a Factor to Arts & Culture Low Attendance Rates of Black/African American and Low-Income Communities in Pittsburgh

## Problem Statement 

In their [2020 “Culture Counts” Report](http://www.pittsburghartscouncil.org/storage/documents/Research/Culture_Counts-2020-Technical_Report.pdf), the Greater Pittsburgh Arts Council (GPAC) found that attendance rates to arts and culture institutes in Pittsburgh have increased, though participation among Black and African Americans has remained poor. According to the report, “[d]espite high participation and ratings overall, the region’s arts and culture sector ranks somewhat lower among Black and African American populations on two key measures: 1) attendance and 2) ratings of the region’s arts and cultural opportunities.”  In addition, the [Pittsburgh Regional Quality of Life Survey](https://www.ucsur.pitt.edu/files/center/qol/2018/Pittsburgh%20Regional%20QOL%20Survey%20Full%20Report_2018.pdf), compiled in 2018 by the University of Pittsburgh Center for Social and Urban Research, reported that “46 percent of residents with incomes under $25,000 visit a museum, gallery or attend an arts of cultural event only two or fewer times a year compared to 21 percent of those earning $100,000 or more who attend that infrequently.”  

This same report found that of those who rely on public transit, meaning they take it at least once a week, the highest percentage (38.2 percent) make under $25,000, and while almost a quarter (23.5 percent) make between $25k and $49.9k.  It also found that “...37 percent of African American residents ride public transit at least 5 days a week, which is more than twice the rate of white residents.  In these same income brackets, almost half (45.1 percent) of those making under 25k, and 42.3 percent of those making 25k to 49.9k, rated public transit as having a “moderate” to “severe” problem.   So, though the data show that attendance rates have increased, the question remains: Who is coming? And, more importantly, who is not coming, and why not? This project will attempt to partially answer that question by revealing potential correlations between arts & culture attendance, public transit, income, and race in Pittsburgh .

Additionally, Pittsburgh is attempting to become a cultural epicenter. Culture Counts reported that “the arts and culture sector draws larger audiences than Pittsburgh’s three professional sports teams.”  A transit system that does not allow equal access to all audiences could represent lost revenue for the arts & culture sector, which has proven to be a driver of Pittsburgh’s economic health. Findings from this project can help city planners decide which transit routes need to be augmented in order to help shrink disparities in arts & culture attendance and, in turn, provide an extra boost to Pittsburgh’s income from the arts & culture sector. 
 
## Approach/Methodology

This project consisted of two parts: building the map and performing analyses. 

### Building the map
1.	Create a map of Pittsburgh in GIS that includes the following layers:&nbsp;  
a.	Pittsburgh neighborhoods, [table of census tracts that make up each neighborhood](https://www.ucsur.pitt.edu/files/center/qol/2018/Pittsburgh%20Regional%20QOL%20Survey%20Full%20Report_2018.pdf) obtained from University of Pittsburgh University Center for Urban and Social Research (USCSR)&nbsp;  
b. [Pittsburgh demographics (race and income) by census tract](https://data.census.gov/cedsci/table?q=Income%20and%20Earnings&t=Income%20and%20Poverty&g=0500000US42003,42003.140000&tid=ACSST1Y2019.S1901&hidePreview=true), obtained from census.gov&nbsp;  
c.	[Pittsburgh arts & culture attendance rates, by census tract](https://www.ucsur.pitt.edu/quality_of_life_2018.php)).&nbsp;  
&nbsp;  &nbsp;  i.	Information provided by USCSR from their 2018 Pittsburgh Quality of Life survey. The dataset was provided as a set of responses to all questions in the survey. I created a dataset for use in the project by using the responses to the questions: “During the past year, about how many times have you visited a local museum or gallery or attended an art or cultural event, such as a play, concert, festival, reading or film? Responses were recorded in the following ranges: None, 1-2, 3-5, 6-10, 11-20, 21 or more. The final table contained 1851 from 365 census tracts in Allegheny County. I calculated and added fields Average Attendance Rate Per Tract and Number of Positive Responses per Tract

2.	Create a network dataset and service area using publicly available information on Pittsburgh’s transit [General Transit Feed Specification](https://www.portauthority.org/business-center/developer-resources/) (GTFS), data obtained from portauthoritgov&nbsp;  
a.	Facilities used in the service area are Pittsburgh)’s arts and cultural organization. A list of these was provided by SMU Data Arts, an organization dedicated to analyzing impact of arts and culture in US cities. 

### Analyses
1.	Calculate the distance and travel times from/to an art/culture institute by building a public transit network dataset from GTFS data and [street centerlines](https://data.wprdc.org/dataset/allegheny-county-addressing-street-centerlines), downloaded from data.wprdc.org.&nbsp;  
a. Creating a network dataset was not something covered in class. I followed the [tutorial](https://pro.arcgis.com/en/pro-app/help/analysis/networks/create-and-use-a-network-dataset-with-public-transit-data.htm) on pro.arcgis.com to complete this step. 
2.	Create service area rings using arts organizations as facilities. Use this to predict estimated arrival time by public transit.&nbsp;  
3.	Determine correlation between race, income, and accessibility&nbsp;  
a.	Spatially join attendance census tracts to service area. Use attendance rate and frequency of attendance to determine if the two variables are correlated.&nbsp;  
b.	Spatially join race and income layer to the network layer. Use Black/AfroAm population ratio and median income to arts & culture organization arrival times to determine if there is a correlation. 

## Process Log
### Create Race and Income tracts and tract centroids based off census tract shapefile, and census race and income info &nbsp;  
1.	From data.wprdc.org download Allegheny census tracts shapefile, from ucsur.pitt.edu download Pittsburgh neighborhoods by census tract table, and from data.census.gov download Allegheny County race and income tables&nbsp;  
2.	Add Allegheny census tracts&nbsp;  
3.	Add race and household income info&nbsp;  
&nbsp;  a.	Clean data
&nbsp;  &nbsp;  i.	Race data: Remove all columns except total population and race categories and extract Tract ID numbers from Census Name column&nbsp;  
&nbsp;  &nbsp;  ii.	Household income data: Remove all columns except those with GEO_ID, Census Tract Name, and Household data. Extract Tract ID numbers from Census Name column&nbsp;  
&nbsp;  b.	Table join race and income to Allegheny County Census Tract based on TractID&nbsp;  
4.	Create RaceandIncome Tract Centroids&nbsp;  
&nbsp;  a.	Table join Pittsburgh neighborhoods table to Allegheny Census tracts on Tract Number
&nbsp;  b.	Use Layer from Selection to create Pittsburgh Census Tracts. Convert numbers from text to number by creating new fields as Double and using Field Calculate to copy values. Fill null values with 0. Convert percent household income to number of households per income by calculating each percent by total household.
&nbsp;  c.	Create Race/Income tract Centroids using the Feature to Point tool.

### Create Pittsburgh neighborhoods by race and income&nbsp;  
1.	Create PittsburghNeighborhoods using dissolve tool and Select SUM on race categories and income categories to get the households and race for each neighborhood. This will also add PittsburghNeighborhoods feature class to the geodatabase to make permanent.&nbsp;   
2.	Copy this layer – rename one PittsburghNeighborhoodIncome and the other PittsburghNeighborhoodRace.&nbsp;   
3.	Visualize PittsburghNeighborhoodRace: Black/AfroAm Pop ratio as a chloropeth map and visualize PittsburghNeighborhoodIncome household median income as a chloropeth map.

### Add arts organizations and attendance data&nbsp;  
1.	Add Pittsburgh Attendance data table and join to PittsburghCensusTracts on GEOID. Export features and rename TractsAttendance to make join permanent. &nbsp;  
2.	Use Feature to Point tool to create tract centroids from TractsAttendance.&nbsp;   
3.	Import Pittsburgh arts and culture organizations as a point layer using the latitude and longitude from the dataset. &nbsp;  

### Create network dataset with Pittsburgh bus data and street centerlines&nbsp;  
1.	Add Allegheny county street centerlines as a line layer and add RestrictPedestrians as text field in attribute table and ROAD_CLASS as short int field. Use the Intersect tool to pare down Allegheny county streets to a layer called PittsburghStreets.&nbsp;  
2.	Create new geodatabase called BusTransit and add a new feature dataset by importing Pittsburgh GTFS data into a Feature Dataset using GTFS to Network Dataset Transit Sources Tool.&nbsp;   
3. Copy PittsburghStreets into to BusTransit gdb.&nbsp;  
4. Add the bus stops from the GTFS data to the street layer with the Connect Network Dataset Transit Sources to Streets tools.&nbsp;   
5. Create the network dataset from the TransitNetworkTemplate XML file downloaded from the tutorial link.  Run Build Network tool using the new network dataset. 

### Create Service Areas with Arts organizations&nbsp;  
1.	Create a service area using Network Analysis Service Area. Mode should be set to “Public Transit Time.” Set cutoff times to 5, 10, 20, 30, and 60. Set the Date and Time section to Saturday at 2pm, a popular visitation time at arts and culture organizations.&nbsp;   
2.	Import arts and culture orgs as facilities using the Import Facilities button and adding points from the Pittsburgh arts organizations point layer.&nbsp;  
3.	Run the service area.&nbsp;  

## Analyses, Questions, and Solutions

### Question 1: Does time it takes to get to an arts locale by public transit impact attendance?&nbsp;  
<img width="600" alt="scrnsht1" src="https://user-images.githubusercontent.com/73584997/113475753-787c5300-9445-11eb-9eca-cd7cbed6f530.png">&nbsp;  
My first observation was that all of the arts organizations seemed to be generally clustered on the east side of Pittsburgh. Because of this, I expected that the attendance rate would also be much higher in that region.&nbsp;  

<img width="600" alt="scrnsht2" src="https://user-images.githubusercontent.com/73584997/113475757-7ca87080-9445-11eb-8ef7-2f3f3159abec.png">&nbsp;  
A visual comparison of the attendance rate (represented by graduated symbols) and location of arts orgs did not yield any clear results, so I decided to do a simple “Select by Attribute” to see where average attendance rate was 10 times per year or higher.&nbsp;  


<img width="600" alt="scrnsht3" src="https://user-images.githubusercontent.com/73584997/113475762-816d2480-9445-11eb-9198-8342ddfc65c7.png">&nbsp;   

The results of the “Select by Attribute” where average attendance rate was 10 times per year or higher yielded a clearer correlation. The attendance rate is higher in the general vicinity of the cluster of arts organizations, which is unsurprising. It makes sense that those who are closer will attend more often. 

Since my main concern was finding out how accessibility by public transit affects attendance, I used the Service Area I created from the Pittsburgh Public Transit system and ran it with a cutoff of 5, 10, 20, 40, and 60 minutes to find out what attendance was like for those who lived within those travel times. I spatially joined the TractsAttendanceCentroids layer to the service area.&nbsp;   

![Layout1](https://user-images.githubusercontent.com/73584997/113475929-2be54780-9446-11eb-834f-8daaa455a116.jpg)&nbsp;  
This showed that almost every neighborhood in Pittsburgh had accessibility to an arts organization within at least 60 minutes. The exceptions were the neighborhoods Hays and New Homestead.

I examined the attribute table and ran the Table to Excel tool to show the results. The last row, 40 to 60 minutes, only has 4 responses, meaning the survey most likely received less and less responses the farther from the center of Pittsburgh. The row can be disregarded as there were only 4 responses, so it is not representative. 
The attendance rate is calculated using those who attended at least once.&nbsp;   
<img width="700" alt="table1" src="https://user-images.githubusercontent.com/73584997/113475973-65b64e00-9446-11eb-8a5f-b4e9675045fa.png">&nbsp;  
This shows that the longer the time it takes by bus, the lower the attendance rate in general. However, the attendance rates between 0-5 minutes and 0-10 minutes are similar, as are the rates between 10-20 minutes and 20-40 minutes. 

I decided to rerun the service area, this time with cutoff times of 10, 20, 30, and 40. 
<img width="700" alt="table2" src="https://user-images.githubusercontent.com/73584997/113475996-7cf53b80-9446-11eb-935f-b77683f2b962.png">&nbsp;   
These results showed that there was a drop in attendance rate of around 7 to 10 percent for places that were more than 10 minutes away by public transit.&nbsp;   

<img width="700" alt="table3" src="https://user-images.githubusercontent.com/73584997/113476002-88486700-9446-11eb-83a4-fa6896d3d749.png">&nbsp;   
I then calculated the rate of attendees by category of how many times a year they attended. It appears that those who live closer by public transit are more likely to attend more than 10 times a year.&nbsp;  


### Question 2: Are people in Black/Afro American and/or low-income areas farther from arts organizations on public transit? Does this affect their attendance?&nbsp;  

My next question had to do with the correlation between race and income and accessibility to arts organizations. 

I answered this question by joining the attribute table from the Race/Income Tract centroids to the TractAttendance centroids. With the Service Area cutoff still set to 10,20,30, and 40 minutes, I did a spatial join once again of the tract centroids to the service areas.&nbsp;  


![Layout2](https://user-images.githubusercontent.com/73584997/113475930-2daf0b00-9446-11eb-934b-9e337c08ed4f.jpg)
From looking at the map, it appears that there could be a correlation between distance and Black/Afro American populations. Areas that are more heavily Black/Afro American appear to be slightly farther from arts organizations by public transit. I also noticed there are some neighborhoods that fall outside of the service areas: Chartiers City, Banksville, Lincoln Place, and Homestead, however, these neighborhoods do not appear to have a notable Black/AfroAm population ratio.&nbsp;  


![Layout3](https://user-images.githubusercontent.com/73584997/113475932-2e47a180-9446-11eb-8b14-e096d462e844.jpg)
Looking at the map with Household Median Income symbolized using graduated colors and graduated symbols, it appears that there is a slight correlation between income and time by public transit to an arts organization. The neighborhoods that are not within the service area, Chartiers City, Banksville, Lincoln Place, and Homestead, do not appear to have a notably low median income.&nbsp;  


I looked at the attribute table to get a clearer understanding of the data and exported it to Excel for formatting.&nbsp;  
<img width="700" alt="table4" src="https://user-images.githubusercontent.com/73584997/113476041-c3e33100-9446-11eb-9ce9-156ac95d4294.png">&nbsp;   
This shows that the population percentage of Black/AfroAmericans is highest in the 10-20 and 30-40 minute service areas. It is lowest in the 0-10 minute service area. There is also a higher mean income in the 0-10 minute service area, though the median income is around 50k for all service areas except the 10-20 minute service area, which is only about 40K.&nbsp;   

<img width="700" alt="table5" src="https://user-images.githubusercontent.com/73584997/113476053-d6f60100-9446-11eb-8ef7-1360331a94c8.png">&nbsp;   
Comparing the race and income table with the attendance table, the service area from 0 to 10 minutes by public transit has the highest attendance rate, the lowest Black/AfroAm ratio, and the highest mean HH income. 


## Findings and Future Work

The aim of this project was to determine correlation between arts attendance, public transit, Black/AfroAm race, and income through geospatial analysis. Initial observations showed that people who were closer to public transit visited more often. The analyses also revealed that in the service area 0 to 10 minutes from an arts institute, the population is less Black/AfroAm, has a higher mean income, and has a higher attendance rate. The service areas after 10 minutes dropped almost uniformly in attendance rate and the Black/Afro American population increased as a whole.

Though there is some correlation between the variables, after the 0 -10 minute service area the relationship between the variables is no longer meaningful. There is not enough evidence to definitively say that the relationship between the variables is causal and that poor access to arts and culture organizations on public transit negatively impacts attendance of Black/African American or low-income households. However, it does appear that for those who attend arts organizations, distance by public transit affects how often they will attend – those who are closer will visit more frequently.


This project yielded some interesting findings. One that I think is worth researching more in future is the initial observation that most of the arts and cultural organizations are clustered on the east side of Pittsburgh. This is most likely the heart of Pittsburgh’s cultural district, specifically created to generate money for the city through local and non-local tourism. It could be useful to use the Pittsburgh public transit network dataset to calculate the how difficult and/or costly it is to traverse the city of Pittsburgh on public transit as a tourist would. The results of such a study could help city planners make decisions about if shuttles or additional bus lines are needed. 

## Data Sources

Allegheny county zip codes:
 -	https://catalog.data.gov/dataset/allegheny-county-zip-code-boundaries-9a066

Allegheny county census tracts:
 -	https://www.census.gov/cgi-bin/geo/shapefiles/index.php?year=2010&layergroup=Census+Tracts

Allegheny county street centerlines
 - https://data.wprdc.org/dataset/allegheny-county-addressing-street-centerlines

ArcGIS public transit network dataset tutorial:
 - https://pro.arcgis.com/en/pro-app/help/analysis/networks/create-and-use-a-network-dataset-with-public-transit-data.htm

Pittsburgh neighborhoods:
 -	https://ucsur.pitt.edu/files/census/UCSUR_SF1_NeighborhoodProfiles_July2011.pdf

Pittsburgh demographic information (race and income)
 -	Income (by census tract): https://data.census.gov/cedsci/table?q=Income%20and%20Earnings&t=Income%20and%20Poverty&g=0500000US42003,42003.140000&tid=ACSST1Y2019.S1901&hidePreview=true
 -	Race (by census tract):
https://data.census.gov/cedsci/table?q=race&t=Income%20and%20Poverty&g=0500000US42003,42003.140000&tid=ACSDT1Y2019.B02001&hidePreview=false

Pittsburgh arts & culture attendance data: 
 -	Attendance data provided by University of Pittsburgh University Center for Urban and Social Research 2018 Quality of Life Survey (https://www.ucsur.pitt.edu/quality_of_life_2018.php)

Pittsburgh arts & culture organizations
 -	List of arts and culture organizations provided by SMU data arts (https://www.culturaldata.org/)

Pittsburgh GTFS data:
 - https://www.portauthority.org/business-center/developer-resources/


## References

“Create and use a network dataset with public transit data.” Pro.ArcGIS.com. Accessed 1 December 2020. 
https://pro.arcgis.com/en/pro-app/help/analysis/networks/create-and-use-a-network-dataset-with-
public-transit-data.htm)

Gorr, Wilpen L. and Kristen S. Kurland. Pittsburgh Neighborhoods. November 30, 2017. Found in GIS Tutorial for 
ArcGIS Pro: A Platform Workbook, Tutorial 5-3. Redlands, California: Esri Press, 2017.

Greater Pittsburgh Arts Council. 2020 Greater Pittsburgh Culture Counts Technical Report. Pittsburgh, 2020. 
Accessed November 2020.  http://www.pittsburghartscouncil.org/storage/documents/Research/
Culture_Counts-2020-Technical_Report.pdf

SMU Data Arts. Data, resources, and insights for the Arts. https://www.culturaldata.org/.

U.S. Census Bureau, 2019 American Community Survey. Income in the Past 12 Months (in 2019 Inflation-Adjusted 
Dollars). Distributed by Data.census.gov. https://data.census.gov/cedsci/table?q=Income%20and%20
Earnings&t=Income%20and%20Poverty&g=0500000US42003,42003.140000&tid=ACSST1Y2019.S1901&hidePreview=true

U.S. Census Bureau, 2019 American Community Survey. Race. Distributed by Data.census.gov. 
https://data.census.gov/cedsci/table?q=race&t=Income%20and%20Poverty&g=0500000US42003,42003.140000&tid=ACSDT1Y2019.B02001&hidePreview=true

University of Pittsburgh University Center for Social and Urban Research. The Pittsburgh Regional Quality of Life 
Survey, Pittsburgh, December 2018. Accessed November 2020.  https://www.ucsur.pitt.edu/files/center
/qol/2018/Pittsburgh%20Regional%20QOL%20Survey%20Full%20Report_2018.pdf.

Western PA Regional Data Center.  2010 Census Block Groups. May 23, 2018. Distributed by Western Pennsylvania 
Regional Data Center. https://data.wprdc.org/dataset/2010-census-block-groups.

Western PA Regional Data Center. Allegheny County Zip Code Boundaries. February 28, 2018. Distributed by 
Data.gov. https://catalog.data.gov/dataset/allegheny-county-zip-code-boundaries-9a066.

Western PA Regional Data Center. Port Authority of Allegheny County Transit Stops. October 31, 2019. Distributed
by Western Pennsylvania Regional Data Center. https://data.wprdc.org/dataset/port-authority-
of-allegheny-county-transit-stops.


