- https://dba.stackexchange.com/questions/18539/best-way-to-store-units-in-database - the same problem from a database perspective
# Background
## Handling units in a database
- The best practice is to have a single unit of measurement and to use it consistently throughout the application
  - E.g. all lengths must be stored in meters
- If a value with a different unit needs to be input into the database, it should be converted into the base unit within the presentation layer
  - The added bonus to this is that when a user loads values from the database in these base units, their preferences can determine how the unit will
    appear to them  
    - E.g. store all lengths in meters, but user A can display measurements in feet while user B can display measurements in km
## Handling units in Splunk
- I should do the same thing in Splunk, but I can't
  - Splunk is going to ingest data in near real-time and I should store the data that I get from data sources exactly as it is indexed. I risk losing
    some aspects of the raw data if I try to convert related measurements into a single unit
  - Additionally, I can't predict every type of device that a vendor or utility could source data from. Splunk doesn't have a rigid data model because
    you never know what you're going to index. Therefore, I shouldn't treat Splunk like a database in this regard
- Since I can't store everyting in an application-wide unit, I must include a measurement's description term and unit together in its field name
  - E.g. a field name should be "power_MVA", not "power" or "MVA"
  - See the section below for the reasoning
- Storing measurements in different units is okay because I can perform transformations to get all data into the same desired unit when I load data
  from the index
  - This can be done with calculated fields
- In order to do this successfully, I will need very granular sourcetypes
  - E.g. the "scada" sourcetype is too broad. What if I have RTUs from two different vendors, where one RTU provides V readings while the other
    provides kV readings? I will need a sourcetype for each brand and model of RTU. Each sourcetype will have fields names that include the units
    - I would compare bewteen RTU measurements in different units by creating a new calculated field for each sourcetype in the unit that I wanted
# Field name should include unit of measurement
- I should include the unit of measurement as part of the field name. Doing anything else requires too much conceptual effort and doesn't add any
  value that I can discern
## Should the unit of measurement go in a field name?
- The problem with putting a unit inside of a field is that now the field contains two separate pieces of information: the quantity and how it's
  measured
  - E.g. volume is a very general term. It can be measured in:
    - Solid:
      - SI: cubic meters (m^3)
      - Imperial: cubic feet (ft^3)
    - Liquid
      - SI: liter (L)
        - 1 L = (10 cm)^3 = 1000 cubic centimeters = 0.001 cubic meteres
      - Imperial: gallon (gal)
        - There is no clean coversion between gallons and ft^3. Argh!
  - Imperial units are an abomination
- I could also argue that a quantity without an accompanying unit is a worthless piece of information
  - From this standpoint, a unit should absolutely be a part of a field name. In a sense, the unit is the most useful part of the field name. The
    "description" term is just there to make things more human readable
  - E.g. the field for power measured in MVA shouldn't be "power" or "power_MVA", it should just be "MVA"
    - However, by omitting the word "power", it makes it difficult to understand the context within which the MVA measurement exists
## Considering alternatives
### Lookup
- A lookup is intended to add new, static data to an event (e.g. add the price of an item to an online purchase event)
- A unit or description term is not real data. I can't work with the string "MVA" or the string "power" in data searches and transformations
- I don't think lookups are a good solution to this problem
### Alias
- For example, the "power" field could have the "MVA" alias
  - This is bad because 
### Tag
- Same as above