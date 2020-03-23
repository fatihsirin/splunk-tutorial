- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchTutorial/Basicchart
- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchTutorial/Chartoverlays
- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchTutorial/Tableasareport
# Create a chart
- Use the `chart` command to use field values as row labels via the `BY` clause
- Use the `timechart` command to use time values as row labels and field values as columns via the `BY` clause
# Overlay data
- Just remember that it doesn't make sense to display different types of data the same way. This is where chart overlays can become useful
    - E.g. it doesn't make sense to display conversion rate percentages as bars alongside absolute numbers of views, purchases, and cart additions
- I'm not a fan of overlays, at least as shown in the example. A line usually represents a change over time, but that is not the case with this
  overlay
# Sparkline charts
- Sparkline charts can only be added to searches that use `stats` or `chart`
## Example
```
sourcetype=access_* status=200 action=purchase| chart sparkline(count) AS "Purchases Trend" count AS Total BY categoryId | rename categoryId AS Category
```