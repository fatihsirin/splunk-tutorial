- https://docs.splunk.com/Documentation/Splunk/8.0.3/SearchReference/Makemv
# Background
- This function converts a uni-value field into a multi-value field when it can
- `makemv` basically does the same thing as split, except that it always converts the existing uni-value field into a multivalue field.
  - `split` must be used in an `eval` expression, which requires a new calculated field to be created
    - The new calculated field could just have the same name as the old uni-valued field, which would make `split` behave like `makemv`
# Arguments
# Examples
## Split line into buses
- `sourcetype="oms" | makemv delim="-" "Bus Affected" | lookup bus-locations-lookup-def bus_id AS "Bus Affected" OUTPUTNEW latitude longitude`
```
?
```
- This example is cool because it shows that lookups work with multivalue fields
