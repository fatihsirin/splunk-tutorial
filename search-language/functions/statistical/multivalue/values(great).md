- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Multivaluefunctions#values.28X.29 - `values` documentation
# Examples
## Get distinct values of a field
- `index=electrical_utility_data sourcetype=realtime_oms | stats values(Line_ID) | mvexpand "values(Line_ID)"`
  - The `values` function generates a single multivalue field containing the unique Line_IDs of realtime_oms events. The `mvexpand` command splits the
    multivalue field into distinct events
# Purpose
- `values` returns the unique values of the selected field
  - I can supply multiple fields, but it generates a useless result
- While `values` can be used with `timechart`, no actual graph is generated, so it isn't very useful
# Arguments
## Required
- Exactly one field name
## Optional
- x
