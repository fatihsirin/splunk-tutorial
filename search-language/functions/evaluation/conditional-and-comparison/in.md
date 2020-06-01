- https://docs.splunk.com/Documentation/Splunk/8.0.3/SearchReference/ConditionalFunctions#in.28FIELD.2C_VALUE-LIST.29 - `IN` documentation
- https://answers.splunk.com/answers/74348/replace-a-character-with-a-linebreak-in-table-results.html - closest thing I coud find to using newlines in
  search queries
- https://www.splunk.com/en_us/blog/tips-and-tricks/smooth-operator-searching-for-multiple-field-values.html - various usages of `IN`
# Examples
## Find exact matching value 
### Search
```
index="electrical_utility_data" | where _raw IN ("Output from run_untopchan.sh
Goodbye World!")
```
- The newline within the double quotes is required for this search to work
- I haven't so far discovered how to search for newlines without doing this
# Purpose
- Use the `IN` operator to check if a field contains an _exact_ value
- Wildcards cannot be used with the `IN` operator. Use `LIKE` instead
# Syntax
- `<field> in (<value1>[, <value 2>...])`
# Performance
- I'm guessing that `IN` is more performant than `LIKE`, otherwise I would just use `LIKE` all the time