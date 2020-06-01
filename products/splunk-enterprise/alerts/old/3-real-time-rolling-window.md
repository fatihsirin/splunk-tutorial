- https://docs.splunk.com/Documentation/Splunk/8.0.3/Alert/DefineRealTimeAlerts#Create_a_real-time_alert_with_rolling_window_triggering
# Main example
## File
### X
```
```
## Searches
-
# Purpose
- This type of alert will trigger an action when some criteria are met within the rolling time window in real-time
# Search evaluation workflow
-
# Management workflow
-
# Syntax
-
# Files
-
# Performance
- This type of alert is the most resource intensive of all three
  - I presume it's because it's 1) real-time and 2) must track and update matching events within the time window instead of just triggering when a
    single matching event is found