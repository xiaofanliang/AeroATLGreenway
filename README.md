## Transforming Mobility Barriers to Connectivity: Examining the Impact of the AeroATL Greenway Plan in Reconnecting Aerotropolis Communities Around Atlanta Airport

![Interactive AeroATL Greenway Web tool](Webtool_Demo.png)

[TAKE ME TO THE MANUSCRIPT]().
[TAKE ME TO THE AEROATL GREENWAY WEB TOOL](https://xiaofanliang.github.io/AeroATLGreenway/).

### Project Motivation and Background 

Airports are key stakeholders in mobility networks. They help regions improve global competitiveness and drive local economies. However, at the micro level, an airport is a large and restricted land use that is difficult for the nearby residents to move around and an ‘infrastructure sink’ that congregates resources to serve aviation and related industries alone without considering the needs of the adjacent communities. 

This project uses AeroATL Greenway Plan as a case study to critically examine such a connectivity infrastructure for Aerotropolis residents that contest the mobility barriers posed by H-JAIA airport and its surrounding built environment. We ask two research questions: 1) is H-JAIA a barrier to nearby residents’ mobility, and 2) how well the AeroATL Greenway Plan can improve Aerotropolis residents’ walking and biking trip distance and experience. For the first question, we use origin and destination flow data from Atlanta Regional Commission’s Activity-based Model to quantify and visualize H-JAIA’s barrier effect. For the second question, we model changes in trip distance and trip experience with six network scenarios and host a participatory modeling workshop with community members to discuss and validate scenario modeling results. We work with [Aerotropolis Atlanta](https://aeroatl.org/special-projects/aeroatl-greenway-model-mile/) to understand contexts, set our goals, and engage with stakeholders. 

This project is part of Xiaofan Liang's City and Regional Planning PhD Dissertation at Georgia Institute of Technology. It is funded by (ACRP (Airport Cooperative Research Program) Graduate Research Award)[https://www.trb.org/ACRP/GraduateResearchAwards.aspx]. 

### Data

#### Source (updated by June 22, 2023)
| Data          | Link/Source |
| ------------- | ------------- |
| AeroATL Greenway | [Regional Trails Vision & Bicycle Facility Inventory (ARC)](https://garc.maps.arcgis.com/apps/webappviewer/index.html?id=eb154059fd3943e781539d97292225fa); [Atlanta Region Trail Plan Inventory (ARC)](https://opendata.atlantaregional.com/datasets/GARC::atlanta-region-trail-plan-inventory/explore?location=33.628607%2C-84.389893%2C12.92). The AeroATL Greenway Plan data at ARC is based on the [2018 BluePrint](https://aeroatl.org/wp-content/uploads/2020/04/AeroATL-Greenway_Report-7-4-2018-reduced.pdf). There has been updated [discussions on Model Miles segments](https://aacids.com/wp-content/uploads/2019/07/Appendix-C-Project-Descriptions-and-Context.pdf) but corresponding GIS data are not available yet.|
| Linked Roads | OpenStreetMap (2022). Data is downloaded through R package `osmdata`, with `key = 'highway'` and `value = c('motorway_link', 'motorway', 'primary', 'secondary', 'tertiary', 'residential', 'trunk', 'primary_link', 'secondary_link','tertiary_link','unclassified')` |
| ------------- | ------------- |

#### Downloading Data in the Web Tool


### Potential Problems with the Web Tool

**Please do not query the web tool or use it in a large event (over 100 people)**. Using the web tool for community engagement workshop, Greenway Plan design, or individual exploration are perfectly fine. The web tool relies on a customized API Xiaofan designed and hosted on Google Cloud Run and it will cost her $$$ if there is a large amount of traffic. 

**If the trip summary and geometry in "Trip Scenarios" takes more than 1min (map loading "Updating..." text), refresh the page.** Sometimes there may be a jam to the API call. It may also help to wait a bit and choose a shorter trip. 

**If double click does not show selected origin and destinations, or only one route geometry shows up for the selected OD, refresh the page.** 

**If you have toggled the basemap styles in the Maps tab, refresh the page before you go to Trip Scenario tab.** Otherwise, you may see the weird effect above. This is an implementation bug that I could not solve for now. 

### Reporting Issues or Requesting Updates on the AeroATL Greenway Web Tool
Xiaofan is happy to maintain the web tool with some minimal efforts. To keep track of requests, please [open an issue](https://github.com/xiaofanliang/AeroATLGreenway/issues) on the same Github. If this sounds too technical, please email her at xiaofan.l@gatech.edu with the information requested below. She currently accept the following update requests: 
 
* Updates of building polygon attributes, either on one or a few specific building polygon(s) or with a city-wide updates on parcel land uses. In the issue or email, please tell me the Building ID of the polygons and what you want to change.   

* Adding new data layers in the *Maps* tab. Adding new GIS layer is easy, though too many data may be too crowded to show. In the issue or email, please attach the shapefile and geojson of data you want to add and an explanation of why. 

* Editing Greenway Plan Layer(s) in the *Maps* tab. In the issue or email, please attach the shapefile and geojson of the updated Greenway Plan Layers. 

Other updates can be discussed case by case. Please email Xiaofan at xiaofan.l@gatech.edu for further conversations. 