- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Top
# Usage
- x
# Input
- Accept events returned by a search
# Requried arguments
- At least one field name
# Output
- Return the n most common values for the given field in a data table
# Syntax
- Use the `limit` keyword to control how many results are returned
# Examples
```
sourcetype=access_* status=200 action=purchase | top limit=1 clientip
```