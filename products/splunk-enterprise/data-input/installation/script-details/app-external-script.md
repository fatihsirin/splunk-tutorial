- https://answers.splunk.com/answers/716229/what-are-best-practices-for-referencing-a-scripted.html - get input from a script that exists as part of
  another app
- https://answers.splunk.com/answers/716024/what-are-best-practices-for-deploying-an-add-on-wi.html - best practices for consuming an external script
# Get input from a script that is defined in a different app
## Hacky approach example
### `$SPLUNK_HOME/etc/apps/other_app/default/inputs.conf`
```
[script://./bin/cpu.sh]
disabled = false
interval = 15
source = awesome_source
sourcetype = generic_single_line
```
- This is the configuration defined for an app called "other_app"
- Notice how other_app defines other configuration parameters underneath the stanza that directs other_app to consume from a script "cpu.sh"
- This is perfectly correct and valid. This app has its own script and consumes data from it
### `$SPLUNK_HOME/etc/apps/my_app/default/inputs.conf`
```
[script://$SPLUNK_HOME/etc/apps/other_app/bin/cpu.sh]
```
- This is the configuration defined for an app called "my_app"
- Notice how my_app does not define any other configuration parameters when consuming from the script that is external to it
- This is hacky because the configuration that exists in other_app's `inputs.conf` file does not apply to my_app's `inputs.conf` file. The settings
  are not merged. Therefore, I'll have to redefine all of the configuration parameters again in my_app's `inputs.conf` file (not DRY)
  - Perhaps this doesn't matter if I want to consume a script in different ways in different apps. However, this is unlikely. I'll probably always
    want to call a script with the same configuration, regardless of the consuming app
## Recommended approaches
- None of the recommended approaches are a silver bullet
- For my situation, I think that ...