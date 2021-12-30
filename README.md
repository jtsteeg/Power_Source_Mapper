# Power Source Mapper
## Project description
### What problem does this project solve?
When planning to build a large scale campus (hospital, school, datacenter, airport, etc.), a crucial factor is to know how much electricity will be needed to support the finished structure, and where that electricity is coming from. Now more than ever, decision makers need to be cognizent of the fact that new campuses need to be built in a region with easy access to renewable energy sources.

### How does it solve the problem?
The Power Source Mapper pulls and cleans the most recent powerplant data from the EIA database and displays it on a map to provide a heuristic for decision makers to visually identify the location, capacity, and the sustainability of energy sources in a given area.


## Install and run


### Part I : Data aquisition and Database creation

1. obtain an API key for the EIA database here: https://www.eia.gov/opendata/register.php
2. clone repo 1: https://github.com/jtsteeg/APItoJSON
3. insert EIA API key in line 8 of getFromApi.py
4. run getFromApi.py (takes ~3 minutes to run)
5. create a CosmosDB account here (https://docs.microsoft.com/en-us/azure/cosmos-db/sql/create-cosmosdb-resources-portal)
6. copy the CosmosDB URI and PRIMARY KEY, insert them into createDatabase.py line 7 and 8
7. (optional) change username and password on line 11 and 12 to one of your choosing
8. run createDatabase.py

   
### Part II : Backend creation and database population

1. clone repo 2: https://github.com/jtsteeg/Power_Source_Mapper_API
2. create virtual environment and install dependencies in requirements.txt
3. update app.py lines 14-18 with your CosmosDB data
4. run app.py, keep it running as you proceed
5. return to postToApi.py from repo 1
6. if you did part I part 7, update postToApi.py line 8 and 9 with the same username and password
7. run postToApi.py


### Part III : Frontend
```javascript
code example
```



```python
#pip install x
```   

## Future feature updates
- [x] Add detailed fuel information
- [ ] Replace color coded pins with recognizable icons
- [ ] Add sorting functionality
- [ ] highlight plant details on map click



## License
TBA
