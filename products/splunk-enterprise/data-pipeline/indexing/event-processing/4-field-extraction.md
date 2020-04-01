- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Configureindexedfieldextraction - pointer to source below
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Aboutindexedfieldextraction - start of section on indexed field extraction
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Indexer/Indextimeversussearchtime - index time vs. search time (this is a broad concept that
  affects more than field extraction)
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/Usedefaultfields - detailed definitions of all default index-time fields

# These notes should be a directory
# Index-time vs. search-time field extraction
- At a high-level, Splunk has the ability to add additional fields to event data 
    - The process of adding these fields is called "field extraction"
- Splunk adds these additional fields so that it can more efficiently store and later search the data
- Splunk can perform field extraction at "index time", which occurs between when the data is consumed and when it is written to disk, and at "search
  time", when a search is run and events are collected from the index(es)

## Search-time field extraction
- Since searching is outside of the indexing stage of the data pipeline, this section is not very detailed
- Search-time field extraction is much more flexible than index-time field extraction
    - The flexibility of search-time field extraction is most valuable for machine data because most machine data either does not have structure or has
    structure that changes constantly
- Search-time extracted field are not stored in any index and are only created when compiling search results
## Index-time field extraction
- Index-time field extraction is most valuable when handling highly structured data, such as CSV, TSV, JSON, etc. files
    - Fields in these data sources can reliably mapped at index time
    - `I have yet to see why defining search-time field extraction for this data is not preferred`
- Splunk has several default fields that it must extract at index time (e.g. sourcetype and host). While it is possible to define additional, custom
  fields that should be extracted at index time, it is not recommended to do so
## Costs
- Why is it not recommended? Because each custom field that is extracted at index time has negative impacts
    - Each field increases the size of the searchable index which makes indexing *and* searching take longer
    - Index-time fields are less flexible. Anytime a change is made to these fields, the entire data set must be re-indexed or the issue must be
      managed at search time by tagging events with alternate values (messy)
## Benefits
- It is possible, though not common, for the benefits of index-time custom field extraction to outweight the costs
- x


# Index-time field extraction
## Default fields
- Splunk tags each data point with several default fields as part of the process of transforming a data point into an event
- There are three types of index-time default fields: internal, basic, and datetime
### Internal default fields
- There are several internal fields that the Splunk software uses internally:
    - `_raw`: the original raw data of the event
    - `_time`: the event's timestamp expressed in UNIX time (UTC)
    - `_indextime`: the time the event was indexed in UNIX time (UTC)
        - This is a hidden field
    - `_cd`: the address of the event within its index
        - This is a hidden field
    - `_bkt`: the id of the bucket the event is stored within
- These fields can be searched on (see the source which details all the fields)
- Hidden fields aren't displayed in search results unless renamed or used with `eval`
### Basic default fields
- These fields provide the most basic tagging information that transforms a data point into an event
    - `host`: the hostname or IP address of the network device that generated the event
    - `index`: the name of the index within which the event is located (e.g. "main")
    - `linecount`: the number of lines an event contains before it was indexed
    - `punct`: the punctuation field that was extracted from the event
    - `source`: the name of the file, stream, or other input whence the input came
    - `sourcetype`: the format of the data input whence the event came
        - This is Splunk's version of a data type
    - `splunk_server`: the name of the Splunk server that contains the event (useful in distributed topology)
    - `timestamp`: the time at which the event occured
        - Presumably, this reflects any changes to an inherent timestamp that was in an event, if one existed to begin with
### Default datetime fields
- There are several datetime fields that only exist in an event if the raw data that formed the event had timestamp information to begin with:
    - `date_hour`: the hour in which the event occurred, range [0, 23]
    - `date_mday`: the day of the month in which the event occurred, range [1, 31]
    - `date_minute`: the minute in which the event occurred, range [0, 59]
    - `date_month`: the month in which the event occurred, range [1, 12]
    - `date_second`: the second in which the event occurred, range [0, 59]
    - `date_wday`: the weekday on which the event occurred, range ["monday", "sunday"]
    - `date_year`: the year on which the event occurred
    - `date_zone`: the local timezone of the event
- If the timestamp information for an event was modified at input or indexing time, these fields do *not* represent those changes
    - E.g. a string such as "05:22:21" will be parsed into indexed fields: `date_hour::5`, `date_minute::22`, `date_second::21` and these fields will
      not change regardless of what timezone "5" was generated in and from what timezone the `date_hour` field is searched
    - E.g. timezone conversions performed on the event won't be reflected in these fields
    - E.g. modifying the event timestamp to actually be the time at which the event was indexed won't be reflected in these fields





- There are two types of indexed fields: default fields and custom fields


## Default fields
- The Splunk software automatically adds these fields to every single event
## Custom fields
- I have the ability to create my own fields at index time