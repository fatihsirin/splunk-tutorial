- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Chart
# Description
- Display the statistical function outputs as columns in the table and display the values returned by the split-by clause (i.e. `BY`) as rows
    - If there is no `BY` clause, then `chart` returns exactly one row whose values are aggregations of the column values
- The `chart` command requires that at least one statistical function is supplied to it
# Syntax
- This command is similar to the `stats` command
# Examples
```
sourcetype=access_* status=200 | chart count AS views count(eval(action="addtocart")) AS addtocart count(eval(action="purchase")) AS purchases by
productName | rename productName AS "Product Name", views AS "Views", addtocart AS "Adds to Cart", purchases AS "Purchases"
```