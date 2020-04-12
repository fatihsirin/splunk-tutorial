- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Aggregatefunctions#distinct_count.28X.29_or_dc.28X.29 - distinct_count
  documentation
# Background knowledge
- `distinct_count` can be used with the `timechart` command to identify if there are any field values that should be identical within a time bin, but
  are not
# Arguments
## Required
- One *or more* field names
  - E.g. `... | timechart distinct_count()`
## Optional
- x
# Examples
- x