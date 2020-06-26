- https://docs.splunk.com/Documentation/Splunk/8.0.2/Report/Createandeditreports
# Dispatch directory
- Each time that a report is run, the search head creates a dispatch directory for the report which seems to contain a *lot* of metadata about the
  search that powers the report
  - Is this much metadata also created and stored for every (non-report) search? Where in the filesystem that happen?
## Search ID (SID)
- Each time that a report is run, the search head generates a unique search ID (SID) for the report that is created from the following report
  metadata: report owner username, report runner username, app context, report name, time the report was run, etc.
  - The SID can be encoded in Base64 for safety (on my machine it seems like Splunk always does this)
- The search head creates a dispatch directory under `$SPLUNK_HOME/var/run/splunk/dispatch/` where the name of the dispatch directory is the SID
  - The report name (and other report metadata) must be short in order to avoid generating a dispatch directory name that is greater than 255
    characters (the Linux filesystem filename character limit)