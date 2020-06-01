- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/SearchTimeModifiers - very detailed breakdown of how to specify the time in the
  search bar
- https://docs.splunk.com/Documentation/Splunk/latest/Search/Specifytimemodifiersinyoursearch - less detailed breakdown of same concept
- WHY ON EARTH IS THERE A SEARCH REFERENCE AND A SEARCH MANUAL?!
# Examples
## By default, relative time modifiers snap to the nearest second rounded down
- The following two searches return different results:
  - `index=electrical_utility_data sourcetype=realtime_scada earliest=-3d latest=-2h | sort _time`
    - This search was run on 2020-05-11 22:34:00+00:00
    - This search returns matching events between 2020-05-08 22:34:00+00:00 and 2020-05-11 20:34:00+00:00
  - `index=electrical_utility_data sourcetype=realtime_scada earliest=-3d-1h latest=-2h | sort _time`
    - This search was run on 2020-05-11 22:36:00+00:00
    - This search returns matching events between 2020-05-08 21:36:00+00:00 and 2020-05-11 20:36:00+00:00
- These results prove that relative time modifiers do exactly what they say they do
  - E.g. a time modifier of `-d`, `-1d`, or `-1d@s` limits the time range to exactly 24 hours before the exact time the search was run, not to 12:00
    AM on the day the search was run (e.g. 12:00 AM on 5-8-2020)
## Relative time modifiers are relative to now
- `index=electrical_utility_data sourcetype=realtime_scada earliest=-1d latest=-2h | sort _time`
  - This search was run on 5-11-2020 22:41:00+00:00
  - This search returns matching events between 2020-05-10 22:41:00+00:00 and 2020-05-11 20:41:00+00:00
    - There were no events between 2020-05-10 22:41:00+00:00 and 2020-05-10 23:59:59+00:00, so that's why the earliest results appear to be at 2020-05-11
      00:00:00+00:00
- This demonstrates that both the `earliest` and `latest` time modifiers are relative to the exact moment that the search is run
## The `latest` time modifier defaults to `now`
- The following two searches return the same events:
  - `index=electrical_utility_data sourcetype=realtime_scada earliest=-1d | sort _time | reverse`
  - `index=electrical_utility_data sourcetype=realtime_scada earliest=-1d latest=now | sort _time | reverse` 
- This demonstrates that if the `latest` time modifier is not specified, it defaults to a value of `now`
## The `earliest` time modifier defaults to `1`
- The following two searches return the same events:
  - `index=electrical_utility_data sourcetype=realtime_scada latest=now | sort _time`
  - `index=electrical_utility_data sourcetype=realtime_scada earliest=1 latest=now | sort _time`
- This demonstrates that:
  - `earliest=1` puts the lower time boundary at the start of UNIX epoch time
  - If the `earliest` time modifier is not specified, it defaults to a value of `1`
## The "snap-to" behavior always rounds down
- The following searches return different results:
  - `index=electrical_utility_data sourcetype=realtime_scada earliest=-1d@w0 latest=05/11/2020:19:14:00 | sort _time`
    - This search was run on 2020-05-11 23:14:00+00:00
    - This search returns matching events between 2020-05-10 00:00:00+00:00 and 2020-05-11 23:14:00+00:00 (837 events)
  - `index=electrical_utility_data sourcetype=realtime_scada earliest=-2d@w0 latest=05/11/2020:19:14:00 | sort _time`
    - This search was run on 2020-05-11 23:14:00+00:00
    - This search returns matching events between 2020-05-03 00:00:00+00:00 and 2020-05-11 23:14:00+00:00 (3429 events)
- This demonstrates that if a snap-to time is specified with a time modifier as `<time modifier>@<snap-to unit>`, Splunk _always_ rounds down
  - In other words, the time that will be snapped to is the most recent whole, non-fractional \<snap-to unit> that does not occur _after_ the \<time
    modifier>
    - In this example, `-1d@w0` look back 24 hours from 2020-05-11 23:14:00+00:00 to 2020-05-10 23:14:00+00:00. Since 2020-05-10 00:00:00+00:00 is a
      Sunday (which is represented as `w0`, `w7`, and `@w`), Splunk rounds down from 2020-05-10 23:14:00+00:00 to 2020-05-10 00:00:00+00:00. By
      comparison, `-2d@w0` looks back 48 hours from 2020-05-11 23:14:00+00:00 to 2020-05-09 23:14:00+00:00. The most recent rounded-down Sunday from
      2020-05-09 23:14:00+00:00 is 2020-05-03 00:00:00+00:00, so Spluk rounds down by 6 days!
## Other fun things that are possible but not detailed here
- Multiple time windows can be used for more granular time searching
  - E.g. `(earliest=<time_1> latest=<time_2>) OR (earliest=<time_3> latest=<time_4>)`
