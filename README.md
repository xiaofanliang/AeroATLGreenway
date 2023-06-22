## Transforming Mobility Barriers to Connectivity: Examining the Impact of the AeroATL Greenway Plan in Reconnecting Aerotropolis Communities Around Atlanta Airport

![Interactive AeroATL Greenway Web tool](Webtool_Demo.png)

[TAKE ME TO THE MANUSCRIPT](https://github.com/xiaofanliang/AeroATLGreenway/blob/main/Manuscript_XiaofanLiang_06222023.pdf).

[TAKE ME TO THE AEROATL GREENWAY WEB TOOL](https://xiaofanliang.github.io/AeroATLGreenway/).

### Project Motivation and Background 

Airports are key stakeholders in mobility networks. They help regions improve global competitiveness and drive local economies. However, at the micro level, an airport is a large and restricted land use that is difficult for the nearby residents to move around and an ‘infrastructure sink’ that congregates resources to serve aviation and related industries alone without considering the needs of the adjacent communities. 

This project uses AeroATL Greenway Plan as a case study to critically examine such a connectivity infrastructure for Aerotropolis residents that contest the mobility barriers posed by H-JAIA airport and its surrounding built environment. We ask two research questions: 1) is H-JAIA a barrier to nearby residents’ mobility, and 2) how well the AeroATL Greenway Plan can improve Aerotropolis residents’ walking and biking trip distance and experience. For the first question, we use origin and destination flow data from Atlanta Regional Commission’s Activity-based Model to quantify and visualize H-JAIA’s barrier effect. For the second question, we model changes in trip distance and trip experience with six network scenarios and host a participatory modeling workshop with community members to discuss and validate scenario modeling results. We work with [Aerotropolis Atlanta](https://aeroatl.org/special-projects/aeroatl-greenway-model-mile/) to understand contexts, set our goals, and engage with stakeholders. 

This project is part of Xiaofan Liang's City and Regional Planning PhD Dissertation at Georgia Institute of Technology. It is funded by [ACRP (Airport Cooperative Research Program) Graduate Research Award](https://www.trb.org/ACRP/GraduateResearchAwards.aspx). 

### Data

#### Source (updated by June 22, 2023)
| Data          | Link/Source |
| ------------- | ------------- |
| AeroATL Greenway | [Regional Trails Vision & Bicycle Facility Inventory (ARC)](https://garc.maps.arcgis.com/apps/webappviewer/index.html?id=eb154059fd3943e781539d97292225fa); [Atlanta Region Trail Plan Inventory (ARC)](https://opendata.atlantaregional.com/datasets/GARC::atlanta-region-trail-plan-inventory/explore?location=33.628607%2C-84.389893%2C12.92). The AeroATL Greenway Plan data at ARC is based on the [2018 BluePrint](https://aeroatl.org/wp-content/uploads/2020/04/AeroATL-Greenway_Report-7-4-2018-reduced.pdf). There has been updated [discussions on Model Miles segments](https://aacids.com/wp-content/uploads/2019/07/Appendix-C-Project-Descriptions-and-Context.pdf) but corresponding GIS data are not available yet.|
| Linked Roads | OpenStreetMap (downloaded in 2022). Data is downloaded through R package `osmdata`, with `key = 'highway'` and `value = c('motorway_link', 'motorway', 'primary', 'secondary', 'tertiary', 'residential', 'trunk', 'primary_link', 'secondary_link','tertiary_link','unclassified')` |
| Bike Lane | [Regional Bikeway Inventory (2021) from ARC](https://opendata.atlantaregional.com/datasets/02f4effc3fa949078a2a42f72cb4dc70/explore) |
| Parcel Land Use | [Fulton county tax parcel and land use (2022) from ARC](https://arc-garc.opendata.arcgis.com/datasets/fulcogis::tax-parcels-2022/explore?location=33.843413%2C-84.476950%2C10.90); Fulton county land use codes and land use descriptions matching table can be found [here](https://fultonassessor.org/wp-content/uploads/sites/16/2019/02/Land-Use-Codes-2019.pdf). Clayton county tax parcel and land use data are requested from Clayton county GIS office in 2023. |
| Building Polygons | [Microsoft Maps Building Footprints (2019-2020)](https://github.com/microsoft/USBuildingFootprints). This building dataset is then spatially joined with TAZ shapefiles, land use parcels, and the job count data from Longitudinal Employer-Household Dynamics (LEHD) dataset|
| Traffic Analysis Zones | [ARC Activity-Based Model traffic analysis zone (2020)](https://gisdata.fultoncountyga.gov/datasets/GARC::arc-model-transportation-analysis-zones-2020/explore). This corresponds to TAZ codes in the ARC activity-based model trips |
| Origin-Destination Flows | [ARC Activity-Based Model (2019)](https://atlantaregional.org/transportation-mobility/modeling/modeling/). Specifically, data are taken from tripData.csv file in the model outputs. These data can be requested through ARC website. The trip origin and destination are recorded on the TAZ level. Some other sources for OD data include [SafeGraph census tract to census tract flows](https://github.com/GeoDS/COVID19USFlows) and LEHD [LODES commute data](https://lehd.ces.census.gov/data/#lodes) on the census blocks level|
| Parks | [Greenspace from ARC (2021)](https://opendata.atlantaregional.com/datasets/GARC::greenspace/about) |
| Job Distribution | [Longitudinal Employer-Household Dynamics (LEHD) LODES7 dataset in 2019](https://lehd.ces.census.gov/data/#lodes). This dataset reports number of jobs (and jobs by salary tiers) on the census blocks level. I use ga_wac_S000_JT01_2019.csv.gz file (Version: LODES7, State: Georgia, Type: Workplace Area Characteristics (WAC)|

#### Download Data in the Web Tool
* Buildings: [data/Building_mapbox.geojson](https://github.com/xiaofanliang/AeroATLGreenway/tree/main/data)
* AeroATL Greenway (same as source): [data/Greenway_mapbox.geojson](https://github.com/xiaofanliang/AeroATLGreenway/tree/main/data)
* A manually integrated network with existing bike lanes, Model Miles, and Priority Networks for routing use: [data/Roads_PP_MM_mapbox.geojson](https://github.com/xiaofanliang/AeroATLGreenway/tree/main/data)

### Tech Deck for Web Tool Implementation
* **Mapbox JS**: the main scripting language for web interaction 
* **R**: the main scripting language for routing computation on the Greenway scenarios
* **R `plumber` package**: create a customized API for R codes 
* **R `dodgr` package**: compute route distance and distance breakdown by road types. Seamless integration with OSM data.  
* **Docker**: environment container to preload data and packages for the next step web hosting. 
* **Google Cloud Run**: host for the R API created through `plumber`. It outputs a URL that our web tool can query and receive outputs in time. 

I implement the web tool through Mapbox JS. However, Mapbox JS does not support routing computation on user-customized networks (i.e., the Greenway Plan). Thus, I create a web API (with R `plumber` package) for the same R codes I use in R to calculate trip distance and road type breakdown (through R `dodgr` package), containerize the API through Docker, and host the API on Google Cloud Run (free to host for a small traffic). As such, when users select an origin and destination on the web tool, a request is sent to both the Mapbox JS Direction Plugin and our custom API to retrieve routing geometry and statistics with the existing road networks and the Greenway (i.e., PN scenario), respectively.  

### Potential Problems with the Web Tool

**Please do not query the web tool or use it in a large event (over 100 people)**. Using the web tool for community engagement workshop, Greenway Plan design, or individual exploration are perfectly fine. The web tool relies on a customized API Xiaofan designed and hosted on Google Cloud Run and it will cost her $$$ if there is a large amount of traffic. 

**If the trip summary and geometry in "Trip Scenarios" takes more than 1min (map loading "Updating..." text), refresh the page.** Sometimes there may be a jam to the API call. It may also help to wait a bit and choose a shorter trip. 

**If double click does not show selected origin and destinations, or only one route geometry shows up for the selected OD, refresh the page.** 

**If you have toggled the basemap styles in the Maps tab, refresh the page before you go to Trip Scenario tab.** Otherwise, you may see the weird effect above. This is an implementation bug that I could not solve for now. 

### Reporting Issues or Requesting Updates on the AeroATL Greenway Web Tool
Xiaofan is happy to maintain the web tool with some minimal efforts. To keep track of requests, please [open an issue](https://github.com/xiaofanliang/AeroATLGreenway/issues) on the same Github. If this sounds too technical, please email her at xiaofan.l@gatech.edu with the information requested below. She currently accept the following update requests: 
 
* Updates of building polygon attributes, either on one or a few specific building polygon(s) or with a city-wide updates on parcel land uses. In the issue or email, please tell me the Building ID of the polygons and what you want to change.   

* Adding new data layers in the *Maps* tab. Adding new GIS layer is easy, though too many data may be too crowded to show. In the issue or email, please attach the shapefile or geojson of data you want to add and an explanation of why. 

* Editing Greenway Plan Layer(s) in the *Maps* tab. In the issue or email, please attach the shapefile or geojson of the updated Greenway Plan Layers. 

Other updates can be discussed case by case. Please email Xiaofan at xiaofan.l@gatech.edu for further conversations. 