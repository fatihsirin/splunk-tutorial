- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Stats
# Description
- The stats command is really a keyword that must be followed by one or more real statistical functions
- Each statistical function is independent; they do not affect each other
    - The result of each statistical function is displayed in its own column
# Syntax
## BY clause
- If a `BY` clause isn't used, only one row will ever be returned by the stats command
    - This row is an aggregation over the entire incoming set of events
- If the `BY` clause is used, one row will be returned for each distinct value of the field specified in the `BY` clause
# Examples
```
sourcetype=access_* status=200 action=purchase [search sourcetype=access_* status=200 action=purchase | top limit=1 clientip | table clientip] | stats
count AS "Total Purchased", distinct_count(productId) AS "Total Products", values(productId) AS "Product IDs" BY clientip | rename clientip
AS "VIP Customer"
```
