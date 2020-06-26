- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Sort
# Examples
## Sort events by longitude
### Search
- `| inputlookup go_track_trackspoints.csv | sort (longitude)`
  - I surround "longitude" in parentheses to force `sort` to use "longitude" as the field to sort by
    - If I didn't do this, sort would interpret "longitude" in some really weird way (perhaps as the optional \<count> arguement? IDK) and the results
      aren't correctly returned
# Purpose
- Use the `sort` dataset processing command to sort the returned events
# Arguments
- `sort [<count>] <sort-by-clause>`
## Required
- \<sort-by-clause>
  - The most basic example of this clause is just the name of a field to sort by
    - Be careful: if I only supply a single field name, surround it in parentheses
## Optional
- \<count>
  - A limit on the number of events that should be returned
