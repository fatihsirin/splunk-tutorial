- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Chart
# Description
- The `chart` command is a transforming command that returns results in a table which can then be displayed in a chart
- The `chart` command requires that a statistical function be supplied to it
# Syntax
- This command is similar to the `stats` command
# Examples
```
sourcetype=access_* status=200 | chart count AS views count(eval(action="addtocart")) AS addtocart count(eval(action="purchase")) AS purchases by
productName | rename productName AS "Product Name", views AS "Views", addtocart AS "Adds to Cart", purchases AS "Purchases"
```