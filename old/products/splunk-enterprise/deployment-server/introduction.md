- https://community.splunk.com/t5/Deployment-Architecture/How-to-restart-the-Splunk-servers-in-a-deployment-server-env/td-p/187858
# Example
## Only restart forwarders
### File
#### /opt/splunk/etc/system/local/serverclass.conf 
```
[serverClass:bdp_indexers]
whitelist.0 = 15.200.99.181
whitelist.1 = 15.200.61.12

[serverClass:bdp_forwarders]
whitelist.0 = 15.200.119.160

[serverClass:bdp_forwarders:app:untopchan-_forwarders]
restartSplunkWeb = 0
restartSplunkd = 1
stateOnClient = enabled

[serverClass:bdp_indexers:app:untopchan-_indexers]
restartSplunkWeb = 0
restartSplunkd = 0
stateOnClient = enabled

[serverClass:bdp_indexers:app:TA-alert_manager-_indexers]
restartSplunkWeb = 0
restartSplunkd = 0
stateOnClient = enabled
```
- If `restartSplunkd` is true for a serverclass, then the deployment clients in that server class _will_ restart automatically after receiving new
  configurations from the deployment server
  - In this example, only "bdp_forwarder" clients who have the "untopchan-_forwarders" app deployed on them will be restarted when the deployment
    server sends new configuration
  - If `restartSplunkd` is false for a serverclass, then the deployment clients _won't_ restart when the deployment server sends new configuration
- Thus, if I want "bdp_indexers" server class clients to restart, I can:
  - Set `restartSplunkd` ahead of time so that the indexers will restart when I pull in new changes on the deployment server
    - This is annoying because I'll also have to disable restarting on the bdp_forwarders server class, and _then_ redeploy again _after_ switching
      the deployment settings to get _only_ the bdp_forwarders server class to restart!
      - I never want the indexers and forwarders to be restarted at the same time because events will be dropped
  - Restart the indexers manually
  - ...
# Pulling in new content
- Pulling in new content to apps/ or deployment-apps/ does _not_ automatically reloading the deployment server, at least not fast enough for me
  - A manual `$ ./splunk reload deploy-server` is required