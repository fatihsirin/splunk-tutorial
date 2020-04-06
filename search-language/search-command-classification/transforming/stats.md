- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Stats
# Usage
- x
# Input
- Accept events returned by a search
# Required arguments
- One or more statistical functions
- Each statistical function is independent; they do not affect each other
# Output
- Return a data table 
- Display the statistical function outputs as columns in the table, and group the results by the unique values returned by the split-by clause (i.e.
  `BY`) as rows
  - If there is no `BY` clause, then `stats` returns exactly one row whose values are aggregations of the column values
# Syntax
- x 
# Examples
```
sourcetype=access_* status=200 action=purchase [search sourcetype=access_* status=200 action=purchase | top limit=1 clientip | table clientip] | stats
count AS "Total Purchased", distinct_count(productId) AS "Total Products", values(productId) AS "Product IDs" BY clientip | rename clientip
AS "VIP Customer"
```
