- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Convert#Convert_functions
# Usage
- x
# Input
- x
# Required arguments
- x
# Output
- x 
# Syntax
- x
# Examples
```
sourcetype="scada" line_id="4-9" | convert auto(origin_voltage) auto(destination_voltage) | eval voltage_diff=(origin_voltage - destination_voltage) | sort _time
```