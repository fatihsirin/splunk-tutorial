- https://docs.splunk.com/Documentation/Splunk/8.0.2/Report/Createandeditreports - inline search vs report search panel
# Panel data source
- A panel can display data that is generated from an inline search or a report
## Inline search
- An inline search:
  - Cannot be scheduled (duh, that's only for reports)
  - Will run every time the dashboard is loaded
  - Will use the permissions of the dashboard
- An inline-search-backed panel is more convenient than a report-backed panel because the underlying search can be edited from within the dashboard
  editor
  - Thus, an inline-search-backed panel can make use of a dashboard time input while a report-backed panel cannot
## Report
- A report can be more performant than an inline search because:
  - Report acceleration can be used to make the panel load faster
  - If a report is scheduled, the results of the last scheduled run will be displayed instantly
- I also like the thought of consistency and mapping between reports and dashboard panels
  - However, the inability to use a shared dashboard time picker is very annoying