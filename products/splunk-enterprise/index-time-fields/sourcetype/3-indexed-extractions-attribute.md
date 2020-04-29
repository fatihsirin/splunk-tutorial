- https://docs.splunk.com/Documentation/Splunk/latest/Data/Extractfieldsfromfileswithstructureddata#Props.conf_attributes_for_structured_data -
  discusses how `INDEXED_EXTRACTIONS` can cause duplicate field extraction
- https://docs.splunk.com/Documentation/Splunk/latest/Data/Listofpretrainedsourcetypes#Pretrained_source_types - pretrained source types that use
  `INDEXED_EXTRACTIONS`
- https://docs.splunk.com/Documentation/Splunk/8.0.3/Data/Extractfieldsfromfileswithstructureddata#Caveats_to_extracting_fields_from_structured_data_files
  - forwarders, not indexers, handle `INDEXED_EXTRACTIONS` and related configuration
- https://docs.splunk.com/Documentation/Splunk/8.0.3/Admin/Propsconf#Structured_Data_Header_Extraction_and_configuration - `INDEXED_EXTRACTIONS`
  actual documentation
# Examples
## Relying solely on `INDEXED_EXTRACTIONS`
### File
#### `/Applications/Splunk/etc/apps/untopchan2/local/props.conf`
```
[realtime_scada]
INDEXED_EXTRACTIONS = csv
```
- This behavior is very tricky. While it appears that `INDEXED_EXTRACTIONS` should affect all events of the realtime_scada source type the same, it does not!
  - When data is sourced from a file data input, the attribute will result in custom fields being extracted
- If this configuration existed in production, events from non-file data inputs wouldn't have custom fields indexed while events from file data inputs
  _would_ have custom fields indexed. This would create inconsistencies in events with the same source type!
  - I've tested this! Don't do it!
## Accidental double field extraction for data that can come from script or file
### File
#### `/Applications/Splunk/etc/apps/untopchan2/local/props.conf`
```
[source::run_untopchan.sh]
TRANSFORMS-realtime_scada_sourcetype = match_realtime_scada_sourcetype
TRANSFORMS-realtime_oms_sourcetype = match_realtime_oms_sourcetype

[realtime_scada]
REPORT-scada_base_fields = REPORT-extract_scada_base_fields
EXTRACT-Origin_Bus_ID,Destination_Bus_ID = ^[^,\n]*,(?P<Origin_Bus_ID>\d+)[^\-\n]*\-(?P<Destination_Bus_ID>\d+)
INDEXED_EXTRACTIONS = csv
```
- The "realtime_scada" source type is assigned to events that originate from the source "run_untopchan.sh" _and_ match a regular expression defined in
  "match_realtime_scada_sourcetype"
- Once an event is assigned the source type, its fields will be extracted _at search time_ with the delimiter-based extraction defined in
  "REPORT-extract_scada_base_fields"
- While this type of search-time field extraction works for scripts that output raw data, it fails for CSV file data because it parses the CSV header
  as an event!
  - Tacking on `INDEXED_EXTRACTIONS = csv` is _not_ a solution to this problem
    - If the attribute is set, then events that come from a CSV file will have their fields indexed twice: once at input time and once at search time
      - Events that come from a script would still have their fields indexed once, as desired, since data from a non-file data input is not affected
        by the attribute
- The solution to dealing with the header line of a CSV file is to... just manually delete the line from the line
  - This seems hacky, but filtering events requires a source stanza and a transform stanza just like search-time field extraction, so filtering won't
    work for this case
  - The solution is _not_ to modify the "match_realtime_scada_sourcetype" regular expression to exclude the header. The regex won't be applied to the
    file when it is uploaded, so it doesn't eliminate the header line!
# Purpose
- The `INDEXED_EXTRACTIONS` attribute is shortcut for setting the values of other attributes, including:
  - `FIELD_DELIMITER`
  - `FIELD_HEADER_REGEX`
  - Many more
- These other attributes are those that affect "structured data header extraction and configuration". That is to say that this group of attributes 1)
  only applies to structured types of data (e.g. CSV) 2) only applies to data that comes from file-based inputs (or files that are uploaded via Splunk
  Web) 3) only applies at input time (e.g. when a forwarder watches a file for changes and gets those changes)
  - These attributes do not affect non-structured data, do not affect data that comes from a non-file input, and do not affect data that has passed
    the input phase
# Files
- This attribute only exists within stanzas defined in `props.conf`
- This attribute only has an affect if associated the `props.conf` file is located on a machine that is handling the input phase
  - Forwarders can watch files for changes. They handle the input phase. Therefore, this attribute could have an effect within a `props.conf` file
    located on a forwarder node type
  - Indexers do not watch files for changes. They do not handle the input phase, only the indexing phase. Therefore, this attribute would have no
    effect within a `props.conf` file located on an indexer
    - Indexers do _not_ parse any structured data that is forwarded to them
      - I shouldn't have to worry about this too much. Spunk's partitioning CLI should break up an app's source code into the correct packages for me