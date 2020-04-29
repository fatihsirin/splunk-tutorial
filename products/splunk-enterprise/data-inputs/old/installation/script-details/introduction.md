- https://dev.splunk.com/enterprise/tutorials/quickstart/setsplunkhome/ - $SPLUNK_HOME setup (optional)
- https://docs.splunk.com/Documentation/Splunk/8.0.2/AdvancedDev/ScriptedInputsIntro - script introduction. The details of these notes are *not*
  apparent in this documentation, which is quite annoying
# Introduction
- The purpose of these notes is to describe the workflow for installing a script as a data input
  - These notes are not about how to properly write a script
- These notes are meant to be used in tandem with other notes in the `installation/` directory (e.g. `web-gui-settings/`)
# Script vs. app
- The concept of a script is orthogonal to the concept of a Splunk app
  - A Splunk app is a bundle of configuration settings. A script is just a script
  - An app could consume data from a script, but the script could exist anywhere on the filesystem and be consumed by multiple apps
  - Just because a script *can* be stored within an app's directory does not mean that every script requires an app
# Minimum required configuration
## Script placement
- Place the script in the appropriate directory. According to the default `inputs.conf`, valid directories are:
  - `$SPLUNK_HOME/etc/system/bin/`
  - `$SPLUNK_HOME/etc/apps/$YOUR_APP/bin/`
  - `$SPLUNK_HOME/bin/scripts/`
- Alternatively, a `*.path` file can go in one of the above directories and point to the real script, which may reside anywhere on the file system
### *.path file example
- Don't do this yet. I don't have a consumable script yet
`x.path`
```
x
```
## Choose an `inputs.conf`
- Placing the script is only part of the process. Now that the script has been placed, there *must* be an `inputs.conf` file that references the
  script
- Determining which `inputs.conf` file references the script is determined by the "app context" that is selected
- Whichever app is chosen will create an internal `local/` directory if it doesn't exist, and then create an `inputs.conf` file if it doesn't exist.
  An example of the contents of that `inputs.conf` are shown below
```
[script://$SPLUNK_HOME/bin/scripts/dope_global_script.py]
disabled = false
interval = 5
sourcetype = generic_single_line
```
- It's best to do this selection through the Splunk Web GUI because it does a bunch of stuff for me:
  - Creates the `local/` directory (if it doesn't exist) and the `inputs.conf` file (if it doesn't exist) in the selected app's directory
  - Somehow tells Splunk to start running the script (I'm not sure what underlying configuration starts the behavior but I'll figure it out)
- I suppose I could modify `$SPLUNK_HOME/etc/system/local/inputs.conf` outside of an app, but Splunk discourages this
# Additional configuration
## Splunk Web GUI visibility
- A script does not need to be visible in the Splunk Web interface to be used by Splunk
  - As long as I write the `Command` string to correctly locate the script, it can be used by Splunk
  - However, it is a lot easier when the script is visible!
- A script will automatically be visibile if it is placed in either of the two script directories outside of any app
- A script that is placed inside of an app won't be visible unless the app itself is visible
  - The `$SPLUNK_HOME/etc/apps/<my app>/default/app.conf` file is where the visibility of an app is set
