- https://docs.splunk.com/Documentation/Splunk/8.0.2/AdvancedDev/ScriptedInputsIntro - start of script data source workflow
# Script workflows
## Streaming data
- Splunk can directly index the stdout of the script to enable a data streaming workflow
- Splunk checks to see if the script is running. If it isn't, Splunk will start the script
## Writing to a file
- A script can write to a file, then Splunk can monitor and index this log file
- Splunk itself can launch the script that writes to the log file at specified intervals instead of me writing a cron job to launch that script
# Script setup
- Splunk is aware of scripts because they must be placed in the `$SPLUNK_HOME/etc/apps/<appName>/bin/` folder of the associated app
## Writing to a file
- Place the script in the correct folder
- Create a stanza in the `.../etc/apps/<appName>/default/inputs.conf` file that references the script. This configuration can determine:
    - How often to call the script
- 
### Background
- It is sometimes useful to have two scripts: one that generates output data and another one that calls it with parameters (e.g. arguments needed to
  query a database)