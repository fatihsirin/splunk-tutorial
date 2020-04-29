- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/Createandmaintainsearch-timefieldextractionsthroughconfigurationfiles - a little bit of
  the discussion on search-time vs index-time field extraction
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Indexer/Indextimeversussearchtime - this is a broad comparison of search-time vs index-time in
  general, but there is a little discussion on search-time vs. index-time field extraction
# TLDR
- Custom search-time field extraction is preferred to index-time field extraction in most, but not all, cases
# Index-time vs. search-time field extraction
- At a high-level, Splunk has the ability to add additional fields to event data 
    - The process of adding these fields is called "field extraction"
- Splunk adds these additional fields so that it can more efficiently store and later search the data
- Splunk can perform field extraction at "index time", which occurs between when the data is consumed and when it is written to disk, and at "search
  time", when a search is run and events are collected from the index(es)
# Search-time field extraction
## When to use
- Search-time field extraction is much more flexible than index-time field extraction
  - The flexibility of search-time field extraction is most valuable for machine data because most machine data either does not have structure or has
    structure that changes constantly
- Search-time extracted field are not stored in any index and are only created when compiling search results
## When not to use
- x
# Index-time field extraction
## When to use
- Index-time field extraction is most valuable when handling highly structured data, such as CSV, TSV, JSON, etc. files
  - Fields in these data sources can reliably mapped at index time
  - `I have yet to see why defining search-time field extraction for this data is not preferred`
- Splunk has several default fields that it must extract at index time (e.g. sourcetype and host). While it is possible to define additional, custom
  fields that should be extracted at index time, think carefully before doing so
## When not to use
- Each custom field that is extracted at index time has negative impacts
  - Each field increases the size of the searchable index which makes indexing *and* searching take longer
  - Index-time fields are less flexible. Anytime a change is made to these fields, the entire data set must be re-indexed or the issue must be managed
    at search time by tagging events with alternate values (messy)