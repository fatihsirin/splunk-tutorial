- https://answers.splunk.com/answers/716229/what-are-best-practices-for-referencing-a-scripted.html - get input from a script that exists as part of
  another app
- https://answers.splunk.com/answers/716024/what-are-best-practices-for-deploying-an-add-on-wi.html - best practices for consuming an external script
# What happens when I try to add the same data input multiple times?
- TLDR: Splunk won't let me add a duplicate data input (thank goodness), and even if I do, Splunk doesn't acknowledge that the erroneous configuration
  exists
  - This is good behavior. Since this is the case, what the heck is everyone talking about when they describe consuming an app-external script? Do
    they mean that they installed an app, D, but are configuring the underlying script data input from D inside a different app, C?
- Splunk won't let me add a new script data input via the Splunk Web GUI if that data input was already added in Splunk 
- To get around this, I created a new `inputs.conf` file inside of the "Searching & Reporting" app context
## Example
### `/Applications/Splunk/etc/apps/custom_app_1/local/inputs.conf`
- When I first added the new script "my_script.py," I set its app context to be "custom_app_1"
    - This makes sense because this app is supposed to consume this script
```
[script://$SPLUNK_HOME/etc/apps/custom_app_1/bin/my_script.py]
disabled = false
interval = 30
source = custom_app_1_source_name
sourcetype = generic_single_line
```
- Search the matching data with `source="custom_app_1_source_name" host="Austins-MacBook-Air.local" sourcetype="generic_single_line"`
### `/Applications/Splunk/etc/apps/search/local/inputs.conf`
```
[script://$SPLUNK_HOME/etc/apps/custom_app_1/bin/my_script.py]
disabled = false
interval = 60
source = search_app_source_name
sourcetype = generic_single_line
```
- Search the matching data with `source="search_app_soure_name" host="Austins-MacBook-Air.local" sourcetype="generic_single_line"`
- Even though this configuration is set, nothing appears when I search the above query
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
## Recommended approaches
- None of the recommended approaches are a silver bullet
- Just don't do this. It's dumb