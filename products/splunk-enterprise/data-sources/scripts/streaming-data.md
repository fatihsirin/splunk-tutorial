- https://docs.splunk.com/Documentation/Splunk/8.0.2/AdvancedDev/ScriptedInputsIntro - start of script data source workflow
# How it works
- If the script isn't already running, Splunk starts it. If it is already running, Splunk does not restart it
    - Splunk is aware of scripts because they must be placed in the `$SPLUNK_HOME/etc/apps/<appName>/bin/` folder
- The platform indexes the stdout stream from the script