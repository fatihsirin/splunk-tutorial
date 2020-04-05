- https://docs.splunk.com/Documentation/Forwarder/8.0.3/Forwarder/Configuretheuniversalforwarder - start of entire section on configuring universal
  forwarders
- https://answers.splunk.com/answers/115242/can-someone-please-explain-to-me-why-splunk-universal-forwarder-uses-port-8089.html - universal forwarder
  and port 8089
# Sanity check
- I have confirmed as of 4/4/2020 that these notes are true, especially notes on ports
  - The Splunk Web interface shows that a universal forwarder is contacting the deployment server
- Even the logs confirm that the forwarder and indexer are communicating. Why can't I search any data?
  - IT WORKS!
# Introduction
- If the CLI is used to configure a universal forwarder, the configuration changes are written to `$SPLUNK_HOME/etc/system/local/outputs.conf`
  - Check this file just to be sure
- Be sure to restart the universal forwarder to make these changes take effect
- Whenever I start Splunk Enterprise or the Splunk forwarder via terminal, it shows me which ports are being used. This is *extremely* useful
# Configure the universal forwarder to send data to the receiver
- The universal forwarder must be given the indexer IP and the port that the indexer is listening on
  - The typical port that the indexer is listening on is 9997
## CLI
- `$ cd /Applications/SplunkForwarder/bin`
- `$ ./splunk add forward-server <host name or ip address>:<listening port>`
  - E.g. `$ ./splunk add forward-server localhost:9997`
# Configure the universal forwarder to connect to the deployment server
- If I want to let a Splunk Enterprise indexer manage connected forwarders via Splunk Web, I can configure the universal forwarder to be a "deployment
  client"
  - In this architecture, the Splunk Enterprise indexer is the "deployment server"
- The deployment client must be given the deployment server IP and port that the deployment server is listening on
  - The typical port that the indexer is listening on is 8089
    - Verify this by checking `/Applications/Splunk/etc/system/default/web.conf`
## CLI
- `$ cd /Applications/SplunkForwarder/bin`
- `$ ./splunk set deploy-poll <host name or ip address>:<management port>`
  - `$ ./splunk set deploy-poll localhost:8089`
# Configure the universal forwarder's own managment port
- I already know that the deployment server's management port matters. Does it matter what the management port of universal forwarder is?
  - No
- "Port 8089 is not needed on a forwarder for sending event data out to indexers or for communicating with a deployment server. In the case of
  deployment server, the forwarder initiates contact with the deployment server as a client. Only the deployment server needs to have the 8089
  listening port up."

# Configure the universal forwarder to monitor some data
## CLI
- `$ cd /Applications/SplunkForwarder/bin`
- `$ ./splunk add monitor <file or directory on forwarder filesystem>`
  - E.g. `$ ./splunk add monitor /var/log`
- Configurations made via CLI are stored in `/Applications/SplunkForwarder/etc/apps/search/local/inputs.conf`
  - I don't know why. Maybe I should put all configuration in here just to see if I can get data to be sent?
# Universal forwarder file examples
## `/Applications/SplunkForwarder/etc/apps/search/local/inputs.conf`
```
[monitor:///Applications/SplunkForwarder/austin-test-directory/dummy2.csv]
disabled = false
index = default
```
- This configuration *did* successfully send data from the universal forwarder to the indexer?
# Successful example
## Search
- `source="/Applications/SplunkForwarder/austin-test-directory/dummy2.csv"`
  - THIS REALLY WORKED
- Only new data that is saved to the file since the remote data input was created is being send to the indexer
## Files
### `/Applications/SplunkForwarder/etc/apps/_server_app_one-and-only-server-class/local/inputs.conf`
```
[monitor:///Applications/SplunkForwarder/austin-test-directory/dummy2.csv]
disabled = false
```
- No interval attribute was needed