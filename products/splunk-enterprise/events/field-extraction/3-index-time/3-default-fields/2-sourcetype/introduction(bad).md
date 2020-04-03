- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Whysourcetypesmatter - introduction to sourcetype section
# What does a sourcetype do?
- The `sourcetype` field of a data input controls how Splunk formats the raw data into events







- The field is important because it tells Splunk how the raw data is formated, *not* what the semantic contents of the data are
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