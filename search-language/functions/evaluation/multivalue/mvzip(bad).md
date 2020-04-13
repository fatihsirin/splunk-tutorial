- https://docs.splunk.com/Documentation/SplunkCloud/8.0.2001/SearchReference/MultivalueEvalFunctions#mvzip.28X.2CY.2C.22Z.22.29
# Background
- `mvzip` takes two *already multivalue* fields and ...
# Arguments
## Required
- Two multivalue fields
## Optional
- A delimiter
# Examples
## Create new multivalue field
Bad example
- `sourcetype="oms" | rex field="Bus Affected" "(?<first>\d)-(?<second>\d)" | eval all_buses = mvzip(first, second)`