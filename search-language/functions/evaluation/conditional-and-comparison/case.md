- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/ConditionalFunctions#case.28X.2C.22Y.22.2C....29 
# Background
- `case` can be used with `eval`, `fieldformat`, `where`, and in `eval` expressions
# Arguments
## Required
- At least one pair of pair of arguments, where a pair consists of a comma-separated boolean expression and an associated return value
# Examples
## Output one of three possible values across an assortment of conditions
- `sourcetype="scada" | eval rating=case('Line ID'="5-6" OR 'Line ID'="6-7", 150, 'Line ID'="3-6", 300, 1=1, 250) | sort _time "Line ID"`