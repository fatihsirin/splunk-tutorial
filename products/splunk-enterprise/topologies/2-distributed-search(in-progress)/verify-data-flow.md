# Examine the indexes on the indexers
## CLI approach
- SSH into the desired indexer instance
### Command
- `./splunk list index <index name>` | grep "totalEventCount"
  - This command requires a Splunk username and password unfortunately
### Purpose
- `./splunk list index <index name>` outputs a lot of metadata about the index, but sometimes I'm only interested in seeing how much data an index
  contains
## Splunk Web approach
- Open Splunk Web on the Search Head
### Searches
#### Search by host
- `host="<desired host>"`
  - E.g. `host="ip-172-31-42-16.us-gov-west-1.compute.internal"`
  - To get the correct \<desired host> value, try searching `index=_internal`, then looking up all the values of the `host` field
  - Narrow the time range down to when I think my data should have flowed from the forwarder -> indexers -> search head
#### Search by source type
- `index=* sourcetype="<desired sourcetype>"`
  - E.g. `index=* sourcetype="realtime_scada"`
  - This really does search all indexes (that the user has permission for)
## Troubleshooting
- Be sure all `*.conf` files are correct
  - E.g. make sure no absolute paths are mismatched between the development and deployment environment
  - E.g. ...