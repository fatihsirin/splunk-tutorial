- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Table
# Usage
- x
# Input
- Accept events returned by a search
# Required arguments
- At least one field name
# Output
- Return a data table that only contains the specified fields as table columns
- This command is particularly useful for filtering output so that it can easily be used in a subsearch
# Syntax
- x
# Examples
```
index=usgs_* source=usgs place=*California | table time, place, mag, depth
```