- https://docs.splunk.com/Documentation/Splunk/8.0.2/AdvancedDev/ScriptSetup - file-to-Splunk workflow
# File input vs. stdout input
- Splunk can index the stdout of a script so that everything that is written to stdout by the script goes into Splunk
    - This is NOT the workflow I am describing here
- What I want to do is tell Splunk to monitor and index a regular old log file. It just so happens that a script will be periodically writing to this
  log file
    - In order to do this, I can configure my Splunk deployment to launch the script at specific intervals (instead of running the script with a cron
      job or something)
# Workflow
## Write the script
- ...
## Place the script in the appropriate Splunk directory
- Scripts must go in one of the following directories (according to inputs.conf):
    - `$SPLUNK_HOME/etc/apps/$YOUR_APP/bin/` (recommended)
    - `$SPLUNK_HOME/etc/system/bin/`
    - `$SPLUNK_HOME/bin/scripts/`
    - Anywhere in the filesystem, so long as there is a `*.path` file in one of the above directories that points to its location
- Where should the script write its output files?
    - Splunk doesn't say. 
## Modify the appropriate configuration files
### inputs.conf
- Modify the `$SPLUNK_HOME/etc/apps/<appName>/default/inputs.conf` file to configure script parameters
    - See example file
    - This is NOT the directory with highest precedence since `inputs.conf` is a global context configuration file
### props.conf
- Modify the `$SPLUNK_HOME/etc/system/local/props.conf` file to configure how Splunk will parse the content of the output file to form events
    - See example file
### transforms.conf
- Modify the `$SPLUNK_HOME/etc/system/local/transforms.conf` file to configure how ...
    - TODO: read more on field extraction ...
    - I don't understand the example
    - I'm going on a wild goose chase. I started with the Splunk Knowledge Manager Manual, which pointed me at a bunch of different stuff



# Detecting sc