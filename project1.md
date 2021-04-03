
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

### Question 1: Does time it takes to get to an arts locale by public transit impact attendance?


