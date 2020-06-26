- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Aggregatefunctions#distinct_count.28X.29_or_dc.28X.29
# Examples
## Count distinct field values
- `index=electrical_utility_data sourcetype=realtime_oms | dedup Line_ID | stats count`
  - This search returns a value of 5, since there are 5 unique values for the Line_ID field
- `index=electrical_utility_data sourcetype=realtime_oms | stats distinct_count(Line_ID)`
  - This is a more idiomatic way of getting the same results
# Purpose
- The `distinct_count` statistical function returns the number of unique field values within a field, within a bin
  - E.g. `distinct_count` can be used with the `timechart` command to identify if there are any field values that should be identical within a time
    bin, but are not
# Arguments
## Required
- If no arguments are provided, `distinct_count` returns the distinct count of every field!
## Optional
- Exactly one field name
  - If I pass multiple field names deliniated by spaces or commas, I think the function interprets the arguments as a single field name