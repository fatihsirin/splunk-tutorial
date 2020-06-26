- https://docs.splunk.com/Documentation/Splunk/8.0.2/Report/Createandeditreports - time range picker description
# Time range picker
- A time range picker is an optional addition to a report
- If a time range picker is added, then *only* when the report is viewed (i.e. the report SQL string is not being viewed from within the Search app
  and the report is not being viewed through a dashboard), a green button will appear in the top left corner that will allow the user to change the
  time interval over which the report is run *without* leaving the report view
- The time range picker cannot be added to a scheduled report because a scheduled report always displays the results from the last scheduled run
  - If a report with a time range picker is then scheduled, the time range picker will disappear
## Include the time range picker
- If a time range picker is included with a report, then any user that can view the report can use the time range picker
- A time range picker allows any user to rerun the report over a different time range *without* actually editing the report
## Exclude the time range picker
- If a time range picker is excluded from a report, then nobody can view the report and rerun the report over a different time interval
- However, this doesn't really protect or prevent anything (it's not like there's anything to abuse anyway)
  - Users with write permissions can always just edit the report directly
  - Users without write permissions can open the report SQL string in Search and do whatever they want from there, and they can clone the report into
    a new report
- Thus, excluding a time range picker doesn't "protect" anything. It just makes things less convenient.
  - However, if a report isn't meant to be run over a different time interval, then excluding a time range picker makes that fact more obvious
## Time range picker vs. dashboard time input
- A report *cannot* be controlled via a dashboard time input
- Thus, whether or not a report has a time range picker is orthogonal to using a dashboard time input
