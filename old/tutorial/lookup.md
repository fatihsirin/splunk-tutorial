- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchTutorial/Usefieldlookups
# Introduction
- Lookup files contain data that can be used to understand other data
- Lookup file data do not change often
    - E.g. customer names, customer addresses, product names, employees, equipment, etc.
- Lookup files are also called "lookup tables" or "lookup table files"
- It's annoying that I have to change the permissions for every step of this process, but it's an essential step
# Existing lookup files
- The following lookup files are included with the Splunk software:
    - /opt/splunk/etc/apps/pas_hr_info/lookups/employee_details.csv
    - /opt/splunk/etc/apps/search/lookups/geo_attr_countries.csv
    - /opt/splunk/etc/apps/search/lookups/geo_attr_us_states.csv
    - /opt/splunk/etc/apps/search/lookups/geo_countries.kmz
    - /opt/splunk/etc/apps/search/lookups/geo_us_states.kmz
# Add lookup files
- It is not sufficient to upload a lookup file and share it with one or more apps. A lookup definition must also be created
## Create an automatic lookup definition
### Lookup input fields
- These are fields that the lookup table and the events have in common
    - By "in common", I mean that both the lookup table and the events have two fields that mean the same thing. The fields could have different names
      entirely
- The first box is the field name in the lookup table and the second box is the field name in the events
### Lookup output fields
- These are fields that I want to add from the lookup table to the events
- The first box is the field name in the lookup table and the second box is the name of the field that will appear in the events
# Using lookup files
- The information provided by a lookup file can be used through the `lookup` command, or the field lookup can be set to run automatically
## Automatic usage
- When I run a search, Splunk will automatically add the lookup fields to the interesting fields of the matching events
- 