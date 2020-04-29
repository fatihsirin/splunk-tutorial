- https://docs.splunk.com/Documentation/Forwarder/8.0.3/Forwarder/HowtoforwarddatatoSplunkEnterprise - meat and potatoes of installation
- https://docs.splunk.com/Documentation/Forwarder/8.0.3/Forwarder/Installanixuniversalforwarder - macOS universal forwarder installation
# Introduction
- Yes, the deploy server and the universal forwarder client are communicating with the current settings as of 4/4/20
# Enable the receiver to accept forwarded data
- Modify the `$SPLUNK_HOME/etc/system/local/inputs.conf` file to listen for data on a port
- The typical port to listen on is 9997, but any open port will do
  - Use `netstat` to examine ports if needed
## Example
### `$SPLUNK_HOME/etc/system/local/inputs.conf
```
[splunktcp://9997]
disabled = 0
```
- Restart Splunk Enterprise to apply the changes
# Install the universal forwarder on the desired instance
- Make sure to save the universal forwarder username and password
# Start the universal forwarder for the first time
- Starting and stopping the forwarder application has the same workflow as Splunk Enterprise, it's just that the SplunkForwarder application is a
  separate application with its own directory, binaries, etc.
- After starting it to set it up, stop it to finish the rest of these steps
# Configure the universal forwarder
- Make sure that I'm working with the SplunkForwarder `splunk` command when running these commands, not the Splunk Enterprise `splunk` command
- Tell the the forwarder to forward data with the following commands:
  - `$ cd /Applications/SplunkForwarder/bin`
  - `$ ./splunk add forward-server <host>:<port>`
    - E.g. `$ ./splunk add forward-server localhost:9997`
# (Optional) Configure the universal forwarder as a deployment client
- If a universal forwarder is configured as a deployment client, its configuration can be controlled from a centralized deployment server
  - This is great because I can configure the universal forwarder through Splunk Web on the deployment server
- Any Splunk Enterprise indexer can be a deployment server
  - The deployment server automatically activates when a universal forwarder connects to the indexer management port (by default, that's 8089)
    - Keep in mind that the deployment server (i.e. Splunk Enterprise) and the universal forwarder (SplunkForwarder on my machine) by default will
      both try to use 8089 as their *management* port. Splunk will detect and fix this via CLI prompts
        - I set 8090 to be the management port of the forwarder. The forwarder still correctly polls 8089 which is controlled by Splunk Enterprise)
- Configure the universal forwarder to listen for the deployment server with the following commands:
  - `$ cd /Applications/SplunkForwarder/bin`
  - `$ ./splunk set deploy-poll <host>:<port>`
    - E.g. `$ ./splunk set deploy-poll localhost:8089`
# Configure the universal forwarder to collect data
- See first source for details and other links for this
## Via CLI
- E.g. `$ ./splunk add monitor <path to file or directory>`
  - E.g. `$ ./splunk add monitor /Applications/SplunkForwarder/austin-test-directory/`
    - I don't know why, but monitoring a directory this way modified `/Applications/SplunkForwarder/etc/apps/search/local/inputs.conf`, not the
      SplunkForwarder system `inputs.conf`
## Via configuration files
- `$ cd /Applications/SplunkForwarder/etc/system/local`
- Create an `inputs.conf` in this directory
- Edit the `inputs.conf` as needed
# (Optional) Install add-ons on the universal forwarder
- It can be done (see source)