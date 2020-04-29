- https://docs.splunk.com/Documentation/Splunk/latest/Admin/Inputsconf#Scripted_Input: - acceptable script directories
# Examples
## .sh file example
### Files
#### /Applications/Splunk/etc/apps/untopchan2/bin/run_untopchan.sh
```
#!/usr/bin/env bash
. ~/.pyenv/versions/venv3.6.9/bin/activate && python ~/repositories/untopchan/data_gen_hist_matpower.py
```
- The minimum permissions for the .sh file are 500 (owner read and execute)
  - I assume that since I started Splunk as myself, the Splunk process has my permissions (i.e. owner execute)
#### /Applications/Splunk/etc/apps/untopchan2/local/inputs.conf 
```
[script://$SPLUNK_HOME/etc/apps/untopchan2/bin/run_untopchan.sh]
disabled = 1
index = electrical_utility_data
interval = 90
source = run_untopchan.sh
```
- I don't mess with the permissions of Splunk-generated files
## .path file example
### Files
#### /Applications/Splunk/etc/apps/untopchan2/bin/invoke-untopchan.path
```
/Applications/Splunk/etc/apps/untopchan2/bin/run_untopchan.sh
```
- The minimum permissions for the .path file are 400 (owner read)
#### /Applications/Splunk/etc/apps/untopchan2/local/inputs.conf
```
[script://$SPLUNK_HOME/etc/apps/untopchan2/bin/invoke-untopchan.path]
disabled = 1
index = electrical_utility_data
interval = 90
source = run_untopchan.sh
```
- I don't mess with the permissions of Splunk-generated files
# Purpose
- Splunk listens on the stdout of the script itself
- A scripted input allows me to simulate real-time data input into Splunk
- Both a normal script and a .path file are treated exactly the same. If it seems like they aren't, refesh the web page
# Management workflow
- A scripted input can be created through Splunk web or directly in `inputs.conf`
# Syntax
- Any environment variables will be expanded into their actual values within Splunk
  - If searching for a source, search for the full path (expand the environment variable)
# Files
- A script can be located directly within once of Splunk's approved script directories (see first source)
  - In this case, Splunk will simply invoke the script directly as configured in `inputs.conf`
- Alternatively, a .path file can be located in the same set of directories. The .path file must contain the path to the command to be invoked (which
  may itself exist anywhere file system)