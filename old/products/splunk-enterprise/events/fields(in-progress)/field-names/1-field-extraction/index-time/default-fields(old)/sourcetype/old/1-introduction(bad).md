- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Whysourcetypesmatter - introduction to sourcetype section
# What is a sourcetype?
- Logically, a sourcetype is a Splunk data type
  - Splunk understands how to format raw data into events and how to group knowledge objects based on the sourcetype that is selected for the raw data
- Physically, a sourcetype is a set of configuration settings that are grouped under a stanza whose name is the name of the sourcetype
- The sourcetype field is so important that it is automatically extracted at index time
- There is no such thing as sourcetype inheritance. If a new sourcetype is created from an existing sourcetype, the new sourcetype just copies all the
  unmodified settings of the original sourcetype under its own stanza
# Assigning a sourcetype
- Splunk will usually select the correct predefined sourcetype for incoming raw data
- For specialized data, I can manually select a different sourcetype or even create my own sourcetype
- A sourcetype can even be assigned on a per-event basis instead of the usual per-source basis
- See `2-assign.md` notes













# What does a sourcetype do?
- The `sourcetype` field of a data input controls how Splunk formats the raw data into events







# When to create a new sourcetype?
- A new sourcetype is needed when the data I'm uploading is unusual and therefore requires customized event-processing settings
# When to not create a new sourcetype?
- If I want to separately examine different data points within a single event, then I need to configure Splunk to extract fields from the event at
  search or index time
  - Extracting fields from an event is a separate task from defining a new sourcetype
    - E.g. if I want each number that occurs in a certain position in each line of a CSV file to mean something, assigning meaning to those numbers is
      done through field extraction
# When to assign per-event sourcetypes?
- Usually, a sourcetype is assigned on a per-source basis
    - Each data input gets a single source. Simple
- However, if a data input supplies heterogenous data, then I can assign each event the correct sourcetype