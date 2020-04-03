- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/HowSplunkextractstimestamps - introduction to timestamp extraction section
# Splunk is usually right
- Splunk adds timestamps to events at index time
  - It stores each event's timestamp in the event's `_time` field
- The Splunk timestamp of an event and any timestamps or time-like strings in the raw data of an event are separate things
- Often, Splunk will simply read a time-like string from the raw data and generate the correct equivalent event timestamp, but not always.
  Sometimes, I might have to tell Splunk to use a converstion process to derive the correct value for the `_time` field from the time-like strings in raw data 
  - Splunk does this by choosing the correct `sourcetype` of the newly added data input, then using the settings defined for that `sourcetype` (inside
    of `props.conf`) to parse the raw timestamps. It also just parses raw data for timestamps on its own if the `sourcetype` definition isn't good
    enough
## The `_time` field
- The `_time` field is internally stored as a Unix timestamp in UTC
- It is translated to a human-readable string when Splunk renders search results
  - Splunk automatically displays the human-readable string with regard to the local timezone
    - E.g. If the raw data had the timestamp `2019-01-01 00:00:00+00:00`, then Splunk would correctly assume that it should use this timestamp to set
      the value of the `_time` field for the corresponding event. The `_time` field would internally be stored as `1546300800`. It would be shown to a
      user in the EDT timezone as the string `2018-12-31T19:00:00.000-05:00` or simply as `	12/31/18 7:00:00.000 PM`
        - This can be verified by by running a search, e.g.: `source="hist_rand_scada.csv" host="Austins-MacBook-Air.local" sourcetype="csv" | sort _time | eval mytime=_time`