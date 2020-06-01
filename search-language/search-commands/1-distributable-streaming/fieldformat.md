- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Fieldformat
# Examples
## Format exactly one field
### Search
```
| makeresults annotate=true | eval my_value=1.327, nice_value=5.44 | fieldformat my_value=round(my_value, 2)
```
- `fieldformat` can only operate on a single field
  - I cannot use spaces nor commas to add additional fields
# Purpose
- `fieldformat` changes the appearance of a field
  - I often want to round a floating point number within a table
# Arguments
## Required
- \<field>
  - `fieldformat` can format exactly _one_ field
    - An `eval` expression can operate on multiple fields
## Optional
- x