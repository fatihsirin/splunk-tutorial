- https://docs.splunk.com/Documentation/Splunk/latest/DistSearch/Forwardsearchheaddata - configure search head as forwarder
# Configure search head to be a forwarder
## Files
### X
```
```
### X
```
```

## Searches
-

# Purpose
- In a distributed search topology, all data should be indexed only on indexers to make it easier to keep track of data
- To accomplish this, a search head can be configured to be a forwarder to the indexers
# Search evaluation workflow
-
# Management workflow
- If an app has an `outputs.conf` file, when the app's package is partitioned in the default way into sub-packages, all three sub-packages get the
  `outputs.conf` file
  - I want the particular stanzas in the above example to only exists in the `outputs.conf` on the search head. How do I accomplish that?
    - Input groups will not help with this. Input groups can only group stanzas in `inputs.conf`
    - It appears that configuring an `outputs.conf` file this way  is beyond the scope of app development. This is something that must entirely be
      done by the admin 
# Syntax
-
# Files
-
# Performance
-