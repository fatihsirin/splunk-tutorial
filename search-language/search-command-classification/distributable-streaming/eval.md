- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Eval
# Usage
- x
# Input
- The `eval` command doesn't accept events as input. It just evaluates an expression
# Required arguments
- At least one expression
# Output
- The result of evaluating the expression
  - Evaluate a mathematical, string, or boolean expression and then either:
    - Add a new field to the output that displays the evaluation of that expression
    - Feed the evaluated expression into another command
# Syntax
- `eval <field>=<expression>["," <field>=<expression>]...`
    - Eval always takes a field name and an expression
# Examples
```
sourcetype=access_* status=200 
| stats count AS views count(eval(action="addtocart")) AS addtocart count(eval(action="purchase")) AS purchases by productName 
| eval viewsToPurchases=(purchases/views)*100 
| eval cartToPurchases=(purchases/addtocart)*100 
| table productName views addtocart purchases viewsToPurchases cartToPurchases 
| rename productName AS "Product Name", views AS "Views", addtocart as "Adds To Cart", purchases AS "Purchases"
```
- `eval` is being used in both ways in this example
- Notce how `eval` is able to use renamed columns (e.g. it uses the "purchases" renamed column)
  - I think SQL doesn't allow this