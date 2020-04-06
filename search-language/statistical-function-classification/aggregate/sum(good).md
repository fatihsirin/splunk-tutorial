- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Aggregatefunctions#sum.28X.29
# Background knowledge
- Like many aggregate functions, the `sum` function *must* be used with one of the following transforming (statistical) functions: `stats`, `chart`,
  or `timechart`
  - `sum` cannot be used on its own
# Arguments
## Required
- A field name or `eval` expression
## Optional
- None
# Examples
## Field name
- `sourcetype="scada" line_id="4-9" | stats sum(current) AS current_sum`
```
-------------
current_sum |
-------------
-609.644    |
-------------
```
- For the returned events, sum the values of their "current" field over the specified time range 
- Return the result as a table with a single column "current_sum" with a single value, "-609.644"
## Eval expression
- `sourcetype="scada" line_id="4-9" | stats sum(eval(current * destination_voltage)) AS weird_sum`
```
-----------
weird sum |
-----------
-54.875   |
-----------
```
- For the returned events, sum the values of x over the specified time range, where x = (current * destination_voltage) for every event
- Return the result as a table with a single column "weird_sum" with a single value, "-54.875"