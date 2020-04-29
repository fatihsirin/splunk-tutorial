# String comparison
- `eval y = if("195.2108115165495700"="195.2108115165495700", "true", "false")` evalutates to true. The two strings are lexicographically equal
- `eval y = if("195.2108115165495700"="195.21081151654957", "true", "false")` evaluates to false. The two strings are *not* lexicographically equal 
# Numeric comparison
- `sourcetype="scada" | eval y = if(195.2108115165495700=195.21081151654957, "true", "false")` evaluates to true. The two numerics are numerically
  equal, which is correct even though the second literal doesn't have two trailing 0s
# Cross-type comparison
- `sourcetype="scada" | eval y = if("195.2108115165495700"=195.21081151654957, "true", "false")` raises an error. Splunk does not let me compare
  numeric types against string types