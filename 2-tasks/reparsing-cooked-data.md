- https://community.splunk.com/t5/Archive/Can-cooked-data-be-altered-again-Or-retimestampped/td-p/404280
- https://wiki.splunk.com/Community:HowIndexingWorks - nice diagrams
- https://community.splunk.com/t5/Archive/Cooked-Connection/td-p/378364 - what does cooked SSL mean?
# Examples
## Default indexer configuration
### Files
#### `inputs.conf`
```
[splunktcp]
route=has_key:_replicationBucketUUID:replicationQueue;has_key:_dstrx:typingQueue;has_key:_linebreaker:indexQueue;absent_key:_linebreaker:parsingQueue
acceptFrom=*
connection_host=ip
```
- The `route` attribute can have several values, each of which is delineated by a semicolon
  - Each value has the following syntax: `has_key|absent_key:<key>:<queueName>;...]`
- The above code is the default configuration of an indexer
  - `has_key:_replicationBucketUUID:replicationQueue`: if an incoming event has the key "_replicationBucketUUID", then the event is sent to the
    replication queue
    - Splunk documentation does not say what the "replication queue" is
  - `has_key:_dstrx:typingQueue`: if an incoming event has the "_dstrx" key, then the event is sent to the typing queue
    - The typing queue holds events before the enter the typing stage. The typing stage performs ... 


- IS THIS EVEN THE CAUSE OF MY PROBLEM? I'M NOT SURE 