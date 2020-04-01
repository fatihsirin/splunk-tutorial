- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Handleeventtimestamps - pointer to next page
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/HowSplunkextractstimestamps - start of section on timestamp configuration
# Introduction
- Splunk will extract a timestamp from an event if it exists
    - It uses a complex set of steps to do this
- If an event has no timestamp information, Splunk will assign the event a timestamp at index time