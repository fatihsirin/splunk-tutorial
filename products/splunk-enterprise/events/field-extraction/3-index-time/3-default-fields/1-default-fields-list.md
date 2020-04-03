- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/Usedefaultfields - detailed definitions of all default index-time fields
# Index-time field extraction
## Default fields
- Splunk tags each data point with several default fields as part of the process of transforming a data point into an event
- There are three types of index-time default fields: internal, basic, and datetime
### Internal default fields
- There are several internal fields that the Splunk software uses internally:
  - `_raw`: the original raw data of the event
  - `_time`: the event's timestamp expressed in UNIX time (UTC)
    - See the `timestamp-extraction` notes
  - `_indextime`: the time the event was indexed in UNIX time (UTC)
    - This is a hidden field
  - `_cd`: the address of the event within its index
    - This is a hidden field
  - `_bkt`: the id of the bucket the event is stored within
    - This is a hidden field
- These fields can be searched on (see the source which details all the fields)
- Hidden fields aren't displayed in search results unless renamed or used with `eval`
### Basic default fields
- These fields provide the most basic tagging information that transforms a data point into an event:
  - `host`: the hostname or IP address of the network device that generated the event
  - `index`: the name of the index within which the event is located (e.g. "main")
  - `linecount`: the number of lines an event contains before it was indexed
  - `punct`: the punctuation field that was extracted from the event
  - `source`: the name of the file, stream, or other input whence the input came
  - `sourcetype`: the format of the data input whence the event came
    - This is Splunk's version of a data type
    - It doesn't matter what the actual data semantically means (e.g. a food price CSV is the same as a SCADA CSV)
      - (Actually this isn't necessarily true)
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
  - E.g. modifying the event timestamp to actually be the time at which the event was indexed won't be reflected in these field