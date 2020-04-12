- https://answers.splunk.com/answers/309165/are-field-names-always-case-sensitive-and-field-va.html - case sensitivity
- https://docs.splunk.com/Documentation/Splunk/7.3.1/Data/Configureindex-timefieldextraction - field name syntax 
# Field names
## Field names are case sensitive
- Field names are always case sensitive
## Allowed characters
- Allowed characters are "a-z", "A-Z", "0-9", or "_"
- Field names cannot begin with "0-9", nor "_"
### Examples
- A CSV column header "Destination Voltage (PU)" will be converted by Splunk into the field name "Destination Voltage _PU"
# Field values 
## Field values are case insensitive
- Field values are always case insensitive