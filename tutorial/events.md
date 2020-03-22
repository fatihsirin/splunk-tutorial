- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchTutorial/Aboutthesearchapp
# What is an event?
- Splunk stores data in logical components called events
- Events are composed of many different fields, all (some?) of which are searchable
# Event fields
## Host
- The host of an event is the IP address, fully qualified domain name, or hostname of the network machine from which the event originated
## Source
- The source of an event is the file or directory path, network port, or script from which the event originated
## Source type
- The source type of an event tells me the data type of the event
    - It is usually based on how the data is formatted
- It is used to search the same type of data across different machines
- An example of a source type could be `access_combined_wcookie`