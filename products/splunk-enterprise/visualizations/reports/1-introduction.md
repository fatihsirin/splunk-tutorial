- https://docs.splunk.com/Documentation/Splunk/8.0.2/Report/Aboutreports - start of documentation on reports
# What is a report?
- A report is a saved search or a saved pivot
# Report names should be short
- When a report is run, the search head generates a unique search ID (SID) that is composed of numerous pieces of report metadata. The SID is used as
  the filename under which the report is saved within the `$SPLUNK_HOME/var/run/splunk/dispatch/` directory. The report name should be short because a
  Linux filename cannot exceed 255 characters