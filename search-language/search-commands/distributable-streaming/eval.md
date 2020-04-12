- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Eval - `eval` documentation
# Background
- `eval` has at least two uses: generating calculated fields and generating a boolean result
  - Calculated fields: `eval` can evaluate one or more expressions and then create new field(s) to display the result(s) of the expression(s) for each
    event
    - E.g. `... | eval <new field>=<expression>["," <new field>=<expression>]...`
    - If the new/generated field matches an existing field name, the values in the existing field are overwritten
  - Boolean result: `eval` can evaluate one or more expressions and then feed that true or false result into another function to apply to each event
    - E.g. `... | eval ["stats" | "chart" | "timechart"] <command>(eval(<boolean expression>)) AS <new field>`
    - The result of the other function must be renamed with an `AS` clause
- The single equals sign, "=", is equivalent to the traditional double equals sign, "=="
- Within an `eval` expression, a string literal in single quotes is a field name while a string literal in double quotes is just a string literal 
  - E.g `sourcetype="scada" | eval x = typeof('Power _MVA')` has "x = Number" because the 'Power _MVA' field stores numeric values. However,
    `sourcetype="scada" | eval x = typeof("Power _MVA")` evaluates to "x = String" because "Power _MVA" is just a string literal!x
# Arguments
## Required
- At least one expression that is:
  - mathematical
    - E.g. `... | eval foo=1 + 2`
  - string
    - E.g. `... | eval foo="dude"`
  - boolean
    - This will not work: `... | eval foo=(1=0)`
      - I cannot actually generate a calculated field by using a boolean expression with `eval` alone. If I want to do that, I must use the `if`
        function
    - This will work: `... | eval foo = if(1=0, "equal", "not equal")`
# Examples
## `eval` is never used for filtering events
- `sourcetype="oms" | eval type="line_outage"`
```
---------------------------------------------------------------------------------------------------------
| started_timestamp         | restored_timestamp        | type        | line_id | bus_affected | amount |
---------------------------------------------------------------------------------------------------------
| 2019-01-01 23:45:00+00:00 | 2019-01-02 00:00:00+00:00 | line_outage | 5-6     | 5-6          | 1.0    |
---------------------------------------------------------------------------------------------------------
| 2019-01-01 19:15:00+00:00 | 2019-01-01 19:30:00+00:00 | line_outage | 6-7     | 6            | 1.0    |
---------------------------------------------------------------------------------------------------------
| 2019-01-01 13:45:00+00:00 | 2019-01-01 14:00:00+00:00 | line_outage | 7-8     | 7            | 1.0    |
---------------------------------------------------------------------------------------------------------
```
- For the three returned events, their "type" field is forced to contain the string literal "line_outage"
  - This shows that Splunk accepts a string literal as a valid expression (at least in this context)
- The three modified, returned events are displayed. No filtering occurred
## `eval` will return NULL for an invalid expression
- `sourcetype="oms" | eval type=lineoutage | where isnull(type)`
```
---------------------------------------------------------------------------------------------------------
| started_timestamp         | restored_timestamp        | type        | line_id | bus_affected | amount |
---------------------------------------------------------------------------------------------------------
| 2019-01-01 23:45:00+00:00 | 2019-01-02 00:00:00+00:00 |             | 5-6     | 5-6          | 1.0    |
---------------------------------------------------------------------------------------------------------
| 2019-01-01 19:15:00+00:00 | 2019-01-01 19:30:00+00:00 |             | 6-7     | 6            | 1.0    |
---------------------------------------------------------------------------------------------------------
| 2019-01-01 13:45:00+00:00 | 2019-01-01 14:00:00+00:00 |             | 7-8     | 7            | 1.0    |
---------------------------------------------------------------------------------------------------------
```
- For the three returned events, the `eval` expression `lineoutage` cannot be evaluated. "lineoutage" isn't an expression! As a result, the "type" field
  should have been erased. In fact it was erased!
  - It's still visually displayed in the events table because it was a real field (i.e. an index-time field, search-time field, alias, etc.). It get
    special visual treatment, but that's it (very confusing)
- The three modified, returned events are displayed
## `eval` will return NULL for an invalid expression
- `sourcetype="oms" | eval zzz=lineoutage | where isnull(foobarbaz) and isnull(zzz)`
```
---------------------------------------------------------------------------------------------------------
| started_timestamp         | restored_timestamp        | type        | line_id | bus_affected | amount |
---------------------------------------------------------------------------------------------------------
| 2019-01-01 23:45:00+00:00 | 2019-01-02 00:00:00+00:00 | line_outage | 5-6     | 5-6          | 1.0    |
---------------------------------------------------------------------------------------------------------
| 2019-01-01 19:15:00+00:00 | 2019-01-01 19:30:00+00:00 | loss_of_load | 6-7     | 6            | 1.0    |
---------------------------------------------------------------------------------------------------------
| 2019-01-01 13:45:00+00:00 | 2019-01-01 14:00:00+00:00 | loss_of_gen | 7-8     | 7            | 1.0    |
---------------------------------------------------------------------------------------------------------
```
- For the three returned events, the `eval` expression `lineoutage` cannot be evaluated. "lineoutage" isn't an expression! As a result, the "zzz" for
  each event is erased
  - "zzz" isn't displayed like "type" was because "zzz" was a completely new field, so it doesn't get any special visual treatment (very confusing)
  - The `isnull` functions demonstrate that the "zzz" field was erased; it was never returned. By a similar token, the "foobarbaz" field never existed
    in the first place, so it too is null!
- The three unmodified, returned events are displayed
## Count when expression is true
- `sourcetype="scada" | stats count(eval(origin_voltage=1.0)) AS perfect_origin_voltage BY origin_bus`
```
---------------------------------------
| origin_bus | perfect_origin_voltage |
---------------------------------------
| 1          | 96                     |
| 2          | 39                     |
...
| 8          | 0                      |
---------------------------------------
```
- For the 864 returned events, count the number of events whose "origin_voltage" field had a value of 1.0 (by origin_bus)
- Return a table of statistics that displays each origin_bus and the number of times it had a voltage of 1.0
## Big example
```
sourcetype=access_* status=200 
| stats count AS views count(eval(action="addtocart")) AS addtocart count(eval(action="purchase")) AS purchases by productName 
| eval viewsToPurchases=(purchases/views)*100 
| eval cartToPurchases=(purchases/addtocart)*100 
| table productName views addtocart purchases viewsToPurchases cartToPurchases 
| rename productName AS "Product Name", views AS "Views", addtocart as "Adds To Cart", purchases AS "Purchases"
```
- Notce how `eval` is able to use renamed columns (e.g. it uses the "purchases" renamed column)
  - I think SQL doesn't allow this