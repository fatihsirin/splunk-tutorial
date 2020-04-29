- https://docs.splunk.com/Documentation/Splunk/8.0.3/Data/Extractfieldsfromfileswithstructureddata - very medicore documentation on "structured
  parsing"
# Important considerations! (refactor)
- Files with structured data file formats, like CSV, can have their _header row_ extracted into searchable fields during the _indexing_ phase
- Splunk _automatically_ performs this index-time field extraction via the `INDEXED_EXTRACTIONS` attribute
- However, there are restrictions to being able to use this index-time field extraction technique
  - Only file-based inputs or "one-shot" inputs (those uploaded one time via Splunk Web) can have this index-time field extraction applied to them
    - This field extraction cannot be performed with modular inputs, network inputs, or _any other type of input_
    - As a result, I need to design my app to use the exact input source(s) it will use in production
      - I can't use `INDEXED_EXTRACTIONS` in my app prototype because the production app will not have people uploading files into Splunk (I think)
      - Thinking _far_ into the future, data will either come from a script or HTTP inputs. Therefore, I need to prototype with those input types. Ugh

- Actually I'm not sure about any of this. Is the `INDEXED_EXTRACTIONS` attribute really responsible for this behavior? The documenation doesn't say.
  I'll have to manually try things out to see. Maybe it's _all_ of the attributes mentioned in the documentation...
  - Stop working on partitioning the app
  - Change the app so that it works with network inputs
  - Re-partition the app and see what's changed

# WHen does structured parsing occur?

# What is structured parsing?
- 
- Structured parsing is a sub-phase of the larger parsing phase of the Splunk data pipeline
