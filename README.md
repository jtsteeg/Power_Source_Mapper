# Power Source Mapper
Jacob Steeg's Capstone project for the Microsoft Leap program. Currently deployed on azure: https://www.powersourcemapper.com/
## Project description
### What problem does this project solve?
When planning to build a large scale campus (hospital, school, datacenter, airport, etc.), a crucial factor is to know how much electricity will be needed to support the finished structure, and where that electricity is coming from. Now more than ever, decision makers need to be cognizent of the fact that new campuses need to be built in a region with easy access to renewable energy sources.

### How does it solve the problem?
The Power Source Mapper pulls and cleans the most recent powerplant data from the EIA database and displays it on a map to provide a heuristic for decision makers to visually identify the location, capacity, and the sustainability of energy sources in a given area.

### Approach

This project was performed in 3 steps with the goal of consolidating data from multiple api endpoints into a single database, scrubbing the data, and then displaying it on a webapp in an easy to understand fashion.


#### Part I : Data aquisition and Database creation
Tools used:
<ul>
  <li>Python</li>
  <li>CosmosDB</li>
</ul>
Using Python, I scraped 3 separate EIA endpoints for relevant Power Plant information:
<ol>
  <li>Name</li>
  <li>Laditude/longitude</li>
  <li>annual output in MWH</li>
  <li>Fuels used</li>
  <li>primary fuel used</li>
  <li>determination of the sustainability of the primary fuel used by the definiton set out by the EIA https://www.eia.gov/energyexplained/renewable-sources/ </li>
</ol>

During this process the data was cleaned to prevent listing any powerplants that were not in operation by 2020 (the most recent year data was available), or that were in operation but had an output of 0 MWH or less. This data was then consolidated into a JSON file.
#### Part II : Backend and database population
Tools used:
<ul>
  <li>Python</li>
  <li>Flask</li>
  <li>Azure FUnctions</>
</ul>
An API was created using python with flask and connected to the CosmosDB database. It was created to be the only channel through which the database can be accessed or updated, and the POST endpoint checks new powerplant submissions for data integrity. The JSON containing the power plant data is then uploaded to the DB via the API. JWT tokens are implemented here to maintain that only authorized users have Create/Update abilities, while read abilities can be performed by anyone with access to the endpoint
  
#### Part III : Frontend
Tools used:
  <ul>
  <li>React.js</li>
  <li>Bing maps developer tools</li>
  <li>azure static web apps</>
</ul>
 Bing maps was used to display geographic data on a familiar map interface, with green pushpins representing powerplants using sustainable fuels while red pushpins represent non-sustainable fuel sources. Further plant details are provided in a detailsList to the right of the map.

## Install and run


### Part I : Data aquisition and Database creation

1. obtain an API key for the EIA database here: https://www.eia.gov/opendata/register.php
2. clone repo 1: https://github.com/jtsteeg/APItoJSON
3. insert EIA API key in line 8 of getFromApi.py
4. run getFromApi.py (takes ~3 minutes to run)
5. create a CosmosDB account here (https://docs.microsoft.com/en-us/azure/cosmos-db/sql/create-cosmosdb-resources-portal)
6. copy the CosmosDB URI and PRIMARY KEY, insert them into createDatabase.py line 7 and 8
7. (optional) change username and password on line 11 and 12 to one of your choosing
8. install cosmosdb python dependency: 	pip install --pre azure-cosmos
9. run createDatabase.py

   
### Part II : Backend creation and database population

1. clone repo 2: https://github.com/jtsteeg/power-source-mapper-api-azure-function
2. create virtual environment and install dependencies in requirements.txt
3. update main.py lines 14-18 with your CosmosDB data
4. install azure functions extension for vscode
5. deploy the project to a function app, follow vscode instructions for deployment
6. copy the URL of the deployed azure function trigger in the VScode output, this will be the endpoint
7. return to postToApi.py from repo 1
8. update postToApi.py line 5 and 6 with "<<endpoint>>/login" and "<<endpoint>>/addpowerplants" 
9. if you did part 1 step 7, update postToApi.py line 8 and 9 with the same username and password
10. run postToApi.py



### Part III : Frontend

1. clone repo 3: https://github.com/jtsteeg/power-source-mapper-webapp
2. update api endpoint in app.js line 19 to "<endpoint>/powerplants"
3. (optional) obtain a Bing Maps API key from https://www.bingmapsportal.com/ and update map.js line 45
3. run "npm install"
4. run "npm start"
5. take the webapp URL (i.e "http://localhost:3000/") and add it to the azure function CORs exception list, reload page

## Future feature updates
- [x] Add detailed fuel information
- [ ] Replace color coded pins with recognizable icons
- [ ] Add sorting functionality
- [ ] Add map/list respond on click
- [ ] expand coverage to continental USA



## License
Usage is provided under the MIT License. See LICENSE for the full details.
