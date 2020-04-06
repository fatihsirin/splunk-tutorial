- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Chart
# Usage
- x
# Input
- Accept events output by a search
# Required arguments
- One or more statistical functions
# Output
- Return a data table which can be used for graph visualizations
- Display the statistical function outputs as columns in the table, and group the results by the unique values returned by the split-by clause (i.e.
  `BY`) as rows
  - If there is no `BY` clause, then `chart` returns exactly one row whose values are aggregations of the column values
# Syntax
- This command is similar to the `stats` command
# Examples
```
sourcetype=access_* status=200 | chart count AS views count(eval(action="addtocart")) AS addtocart count(eval(action="purchase")) AS purchases by
productName | rename productName AS "Product Name", views AS "Views", addtocart AS "Adds to Cart", purchases AS "Purchases"
```