- https://docs.splunk.com/Documentation/SplunkCloud/8.0.2001/Knowledge/Managesearch-timefieldextractions - what is \<class>?
# Introduction
- Performing field extraction at search time is usually preferred to extracting fields at index time
  - Search-time field extraction is more flexible than index-time field extraction because irrelevant extracted fields aren't returned for searches
    that don't care about them
  - Search-time field extraction also often results in better performance because under-used extracted fields aren't stored on the disk. It's faster
    to search indexes with less data
  - However, index time field extraction has it's place. After all the `sourcetype`, `source`, and `host` fields are extracted at search time!
- From now on, this section of notes implicitly refers to only search-time field extraction
# Field extraction types
- There are three types of field extraction:
  - Inline field extraction
  - Transform field extraction
  - Automatic key-value extraction
# How to add search-time field extractions
- There are 3 ways to add search-time field extractions:
  - Add with the "Field Extractor" Splunk Web tool 
    - This is the easiest and fastest method
  - Add with the "Field extractions" page in Splunk Web
    - This is good for viewing field extractions, but not as good as the Field Extractor for creating them
  - Directly edit `props.conf`
    - See `3-edit-config-files` notes
- Field extractions created through the Field Extractor or the "Field extractions" page are only available to their creators until shared with others
  via new permissions
# What is the \<class> value in EXTRACT-\<class> (inline) and REPORT-\<class> (transform) field extractions?
- The \<class> value is merely _the name of the field extraction itself_
- Since each \<class> attribute must exist only within a single \<spec>, can multiple specs have field extractions with the same name?
  - Yes. It is perfectly find for different \<specs> to have field extractions with the same name
- A \<class> is not required to follow typical field name syntax (but try to avoid doing this) 
  - Characters outside of a-z, A-Z, 0-9, and "_" are allowed (verified)
## Examples
### Apply multiple transform configurations to a single field extraction
#### `props.conf`
```
[error-test]
REPORT-error_logs=assignZkError, assignIPSPHP, assignSTD, assignBadScript, assignErrorOnly, assignRestartDigest, assignRestartStatus, assignDud
```
- The name of the field extraction is "error_logs". It's as simple as that. 
- "error_logs" is composed of multiple regular expressions, where each transform configuration in `transforms.conf` encapsulates a single regular expression
#### `transforms.conf`
```
[assignDud]
REGEX = (?<error_message>.*)

[assignRestartStatus]
REGEX = \[(?<error_time>\w{3} \w{3} \d+ \d{2}:\d{2}:\d{2} \d{4})\] \[(?<error_type>\w+)\] (?<error_message>.*)
DEST_KEY = MetaData:Sourcetype
FORMAT = sourcetype::restart

etc...
```
- Here are two of the eight transform configurations that comprise the "error_logs" field extraction
