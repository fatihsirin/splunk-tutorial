- https://docs.splunk.com/Documentation/Splunk/8.0.2/Indexer/Aboutindexesandindexers - high-level introduction. The rest of the notes in the section
  are about configuring and optimizing indexes, which is not what I need at this point in time
# Index contents
## Conceptual index contents
- Splunk transforms raw data into events and stores those events in indexes
- Raw data is transformed into events through many processing techniques, including tagging, parsing, and time-stamping the data
## Logical index contents
- A Splunk Enterprise "index" is a conceptual term that is really made up of several underlying components
- An index is composed of flat files that are stored on the filesystem of an indexer 
  - There is no database software of any kind, full stop
- A Splunk Enterprise index typically consists of many "buckets," organized by age
  - A bucket is a filesystem directory that contains a portion of a Splunk Enterprise index
    - A bucket itself contains three types of files: a single "rawdata" file, index files, and other metadata files
### Buckets
- `<Fill in with more detailed notes later on because there are a lot of details!>`
### Rawdata files
- Essentially, the rawdata file contains a compressed version of the raw data that was indexed by Splunk
  - A rawdata file contains event data as well as journal information that the indexer can use to reconsitute the index's index files
### Index files
- Essentially, an index file contains metadata that the indexer uses to search the bucket's event data
- The main index files are tsidx files, but the bucket also contains other types of metadata files
  - A tsidx file is a time-series index file. It stores associations between unique keywords within events in the rawdata and location references to
    those events. When a search is run, the tsidx files are scanned to find the appropriate location references, which are then used to find matching
    events in the rawdata file
# Index types
- There are two types of indexes: events indexes and metrics indexes
## Events index
- An events index can hold any type of data
  - It is the default index type
- This type of index is flexible, but not as performant as a metrics index
## Metrics index
- A metrics index is an optimized index that only holds metric data
- Metrics data is simply highly structured data that Splunk can interact with in an optimized way
  - Specifically, a metric data point has exactly one timestamp but one or more measurements and one or more dimensions
    - Dimensions are just metadata about the metric, e.g. the host from which the metric data point originated
- A metrics index can handle the higher volume and lower latency requirements of metrics data
  - The result is faster performance with less index storage