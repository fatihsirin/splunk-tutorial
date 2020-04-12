- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/ExtractfieldsinteractivelywithIFX - field extractor utility
# Introduction
- The "Field Extractor" utility is a Splunk Web tool that is excellent for allowing me to extract fields without directly modifying any configuration
  files
- Fields can be extracted via delimiters (best for structured data like CSVs) or regular expressions (best for unstructured data like machine logs)
- The process of extracting fields is pretty intuitive, so read the source for more details
# Save a field extraction
## App context
- The app context is set to whatever app was open
- Select the opened app via the dropdown in the top left corner *before* opening the Field Extractor page (not intuitive)
## Permissions
- Interestingly:
  - If a field extraction is deleted, `props.conf` is updated but `transforms.conf` is not 
  - `transforms.conf` just tacks on new `REPORT-*` stanzas without deleting ones that were created by now-deleted field extractions
- New permission changes are always appended to a `local.meta` file. Permission changes don't erase any data in these files
  - Thus, no permission change is ever lost
### Owner permissions
- If a field extraction is saved with owner permissions, then the updated `props.conf` (and maybe `transforms.conf`) are saved within
  `$SPLUNK_HOME/etc/users/<username>/<app context>/local/`
- The file `$SPLUNK_HOME/etc/users/<username>/<app context>/metadata/local.meta` is also modified
### App permissions
- If a field extraction is saved with app permissions, then the updated `props.conf` (and maybe `transforms.conf`) are saved within
  `$SPLUNK_HOME/etc/apps/<app context>/local/`
- The read/write role permissions must also be set when saving a field extraction with app permissions
  - Since I am saving in an app context, these permissions are saved within `$SPLUNK_HOME/etc/apps/<app context>/metadata/local.meta`
### All apps permissions
- If a field extraction is saved with all app permissions, everthing is the same as single app permissions, except
  - The file `$SPLUNK_HOME/etc/apps/<app context>/metadata/local.meta` is modified to export permissions to system (I don't understand the full
    details yet))