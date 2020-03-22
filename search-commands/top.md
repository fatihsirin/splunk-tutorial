# Description
- Return the most common values for the given field
# Syntax
- Use the `limit` keyword to control how many results are returned
# Examples
- `sourcetype=access_* status=200 action=purchase | top limit=1 clientip`