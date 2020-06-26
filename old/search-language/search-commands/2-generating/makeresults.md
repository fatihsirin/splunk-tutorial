- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Makeresults
# Examples
## Generate basic events
### Search
```
| makeresults count=4
```
- This generates four events whose only field is the `_time` field
## Generate rich events
```
| makeresults count=4 annotate=true
```
- This generates four events that have the following fields with the following values values:
  - _raw: NULL
  - _time: date and time that you run the makeresults command
  - host: NULL
  - source: NULL
  - sourcetype: NULL
  - splunk_server: the name of the server that the makeresults command is run on
  - splunk_server_group: NULL
# Purpose
- Use `makeresults` to generate empty events
# Arguments
- `| makeresults [count=<count>] [annotate=<annotate>]`
## Required
- None
## Optional
- \<count>
  - The number of results to generate
    - Defaults to 1
- \<annotate>
  - A boolean indicating whether or not to add more field
# Syntax
- `makeresults` must be the first command in a search