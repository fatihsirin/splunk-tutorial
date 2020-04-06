- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/ConversionFunctions#tonumber.28NUMSTR.2CBASE.29
# Usage
- Use this function to convert string values into numeric value so that I can perform arithmetic on those fields
  - Alternatively, `convert` (a distributable streaming command) could be used. Actually using `conver` is better because I don't have to create new
    field names
# Input
- Accept a field whose string values should be converted into numeric values
# Required arguments
- A single field
# Output
- Return a new field with numeric values
# Syntax
- `... | eval <new field name>=tonumber(<existing field>)`
# Examples
```
... | eval n=tonumber(store_sales)
```