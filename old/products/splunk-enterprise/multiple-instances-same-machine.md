- https://wiki.splunk.com/Community:Run_multiple_Splunks_on_one_machine - instructions for running multiple instances on the same machine
- https://docs.splunk.com/Documentation/Splunk/latest/admin/Serverconf - `server.conf` documentation
# Important configuration
## `$SPLUNK_HOME/etc/system/local/server.conf`
```
[general]
serverName = server-Austins-MacBook-Air.local-Indexer
pass4SymmKey = <password>

[kvstore]
port = 8192
```
### serverName
- Each Splunk instance running on the same machine _should_ have a different `serverName`
  - A conventional format is "server-\<default host>"
    - Therefore, it's important to have unique host values for each instance running on the same machine
- This attribute is useful to identify the Splunk instance
  - This is important for a variety of reasons, including identifying a unqiue Splunk instance within a distributed search topology
  - However, since server class grouping is done with IP addresses, host names, DNS names, or client names (a Splunk-generated unique ID), the
    `serverName` isn't _required_ to be unique (I think)
### kvstore port
- Each Splunk instance must have its own unique kvstore port
- Upon start-up, Splunk will detect if port 8191 is already bound, and will change the kvstore port of the just-started Splunk instance from the
  default of 8191 if needed
## `$SPLUNK_HOME/etc/system/local/web.conf`
```
[settings]
startwebserver = 0
mgmtHostPort = 127.0.0.1:8090
```
### mgmtHostPort
- Upon start-up, Splunk will detect if port 8089 is already bound, and will change the management port (the port used for deployment-server
  communication) of the just-started Splunk instance from the default of 8089 if needed
  - Only the actual Splunk instance servering as the deployment server should be using port 8089
# Important CLI commands
## `./splunk disable webserver`
- Disable the Splunk Web server from running
- I'll want to do this on indexers and forwarders to mimic a real-life environment as closely as possible and to not confuse myself