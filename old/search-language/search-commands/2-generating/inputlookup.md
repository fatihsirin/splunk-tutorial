- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Inputlookup
# Examples
## Load lookup file into table results
### Search
- `| inputlookup bus-locations.csv`
# Purpose
- The `inputlookup` generating command gets data from a lookup table or lookup file and displays the data as if it came from a search query