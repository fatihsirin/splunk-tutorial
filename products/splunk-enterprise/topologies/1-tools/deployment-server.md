- https://docs.splunk.com/Documentation/Splunk/latest/Updating/Aboutdeploymentserver - start of deployment server documenation
- https://docs.splunk.com/Documentation/Splunk/latest/admin/Serverclassconf - `serverclass.conf` documentation
# Purpose
- The primary purpose of the deployment server is to manage large groups of forwarders, but it can do other things
  - I can manage a simple distributed search topology
  - Unfortunately, the deployment server cannot be used to manage indexer clusters or search head clusters
    - A "deployer" Splunk Enterprise instance is used to manage search head clusters
- A deployment server can help manage _multiple_ Splunk Enterprise deployments
- Managing data is completely separate from managing app configuration. The deployment server has nothing to do with managing indexes on deployment
  clients
# Enable deployment server
- There _must_ be at least one directory in the `$SPLUNK_HOME/etc/deployment-apps` directory for the "Forwarder management" link in the Splunk Web
  settings to show anything interesting
  - I can delete all of the directories afterwards and the web GUI will still show me the deployment server page
- `$ ./splunk enable deploy-server`
  - I don't think it's necessary to run this command. The deploy server seems to work by default
- All of these notes are not about specifically configuring a forwarder
# Configure a client
- The first step is to make clients "phone home" to the deployment server before I start grouping them under server classes
- The _client_ must be told to phone home to the deployment sever. Therefore the following configuration and command are done on the _client_ machine,
  regardless of whether the client is a forwarder, indexer, etc.
## Make clients phone home
### Files
#### `$SPLUNK_HOME/etc/system/local/deploymentclient.conf`
```
[target-broker:deploymentServer]
targetUri = localhost:8089
```
- This is the configuration created by the CLI command to make a Splunk instance server as a deployment client
### Commands
- `$ ./splunk set deploy-poll <host>:<port>`
  - E.g. `$ ./splunk set deploy-poll localhost:8089`
- This command tells the deployment client the IP address and port (which should usually be 8089) of the _deployment server_ so that the client can
  contact the deployment server
  - The client's own management port is _not_ considered in this process because the deployment client is not acting as a deployment server!
- This command _does_ work, provided that the deployment server is correctly configured. It can take a few minutes before the client appears to phone home
# Add a serverclass
## Files
### `/Applications/Splunk/etc/system/local/serverclass.conf`
```
[serverClass:all-indexers]
```
- This is the configuration created by the Splunk Web command to create a new, empty server class called "all-indexers"
- I can add the appropriate clients to a server class in Splunk Web
# Deploy an app
- I must insert an app's source code directory into the `$SPLUNK_HOME/etc/deployment-apps` directory of the deployment server, then I can deploy that
  app
- A deployment app is always distributed from the deployment server as a _unit_ to clients of the desired server class(es)
  - Thus, if I'm being really careful, I must first partition my Splunk app into the three physical sub-packages, then each sub-package will be its
    own deployment app
    - The indexer deployment app will be sent to the indexer server class, the forwarder deployment app will be sent to the forwarder server class,
      etc.
  - If I'm not being careful, I won't partition my Splunk app into the three physical sub-packages
    -  I'll just install the entire app as a single deployment app in the deployment server, and every forwarder, indexer, server class, etc. will get
       _all_ of the app's code, even unnecessary code
## Inspect the app on a deployment client
- When a deployment client gets a deployment app, the client stores the app in `?`
- (in-progress)