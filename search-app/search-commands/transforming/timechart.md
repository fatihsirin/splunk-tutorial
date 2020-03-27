- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Timechart
# Description
- This command is similar to the `chart` command except that the values returned by the `BY` clause are used as columns (instead of rows) and the rows
  are always chronologically ordered time values
- Individual columns, not rows, are always used to form time-series data
# Syntax
-
# Examples
- `sourcetype=access_* | timechart count(eval(action="purchase")) by productName usenull=f useother=f`
