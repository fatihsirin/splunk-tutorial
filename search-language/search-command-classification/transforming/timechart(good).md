- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Timechart#Description - timechart documentation
- https://answers.splunk.com/answers/56010/how-to-display-a-chart-with-raw-data.html - timechart with raw data
- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Timechart#Stats_function_options - statistical functions that are compatible with
  `timechart`
# Background knowledge
## What is a bin?
- A bin is a time interval within which to apply the aggregation function, or the `eval` expression, to calculate a single value to store within that
  bin
  - I'm not considering the `BY` clause here
- When I see the column of timestamps returned by the `timechart` command, each timestamp represents the *start* of a bin. The entire bin encapsulates
  the following interval [timestamp, next timestamp)
    - In other words, the bin encapsulates a timestamp and all the time up to, but not including, the next timestamp
# Arguments
## Required
- x
## Optional 
- x
### \<bin-options>
- Defaults to `bins=100`
- A \<bin-option> argument is optional only because a default of `bins=100` is supplied to the `timechart` command. So in reality, \<bin-options> is a
  required argument
  - Now that I understand that \<bin-options> is a required argument, I understand why an aggregation function or `eval` expression is required to be
    used with `timechart`. Within each bin, only a single value can be returned (disregarding the `BY` clause for now, which can get around this)
    because each bin (i.e. an x-coordinate) must contain a single value (i.e. a y-coordinate) to display on the resulting chart. 
- The documentation is confusing. Multiple possible arguments can be classified as a \<bin-option>. Some can be used together (e.g. `bin` and
  `minspan`) while others cannot be used togther (e.g. `bin` and `span`)
#### bin
- `bin`: set the maximum possible number of time groups (a.k.a. "bins"), **X**, to use to group the data
- The actual number of bins used will be the the number that results in the smallest "reasonable" individual bin size
  - In practice, this means that the largest *number* of possible bins *that evenly divides the time range into bins of a reasonable size* less than
    **X** will be used
  - By default, Splunk seems to define a "reasonable" bin size as 10 seconds, 30 seconds, 60 seconds etc. It won't give me a bin size of 18 seconds
    or 33.3143 seconds
    - E.g. for 2 events with a 30-minute time range returned by a search , `bins=100` could actually result in **X** = 60 for the following reasons:
      - If the 2 returned events have 15 minutes between them, then the total time range must be 30 minutes because the second event must also have
        15 minutes after it so everything is even (even though I didn't query any time past the second event)
      - 30 minutes * 60 seconds/min = 1800 seconds
      - Since `bins=100`, 1800 / 100 = 18 seconds, which is not a reasonable bin size
      - The next largest divisor is 60, because 1800 / 60 = 30 seconds, which *is* a reasonable bin size
      - Hence, the resulting number of bins will be 60 (**X** = 60), where each bin is 30 seconds in size
        - In this example, most bins will have null values because there were only 2 events to measure across those 60 time bins!
#### minspan
- `minspan`: set the minimum possible size of each individual bin. The actual chosen individual bin size must be equal to or greater than this size
  - E.g. for the above example `minspan=30s` won't change anything because the bin size was already chosen to be 30 seconds. However, `minspan=31s`
    will cause the chosen bin size to be 60 seconds (with X = 30) because a 1800 / 30 = 60 seconds, where 60 seconds is the next largest reasonable
    bin size up from 30 seconds 
#### span
- `span`: explicitly set the size of each individual bin
  - E.g. `span=14` will force each bin to be 14 seconds in size. Since 1800 % 14 != 0, Splunk returns an extra bin to account for the remainder (**X**
    = 129). This bin size is not "nice" because the first bin starts before the first event actually happened, but Splunk has to do this to make
    everthing even
- `timechart` does *not* accept both the `bin` and `span` arguments. Why? I think it's because `bin` is supposed to be allowed to choose a
  reasonable `bin` size while `span` allows me to explicitly set it
- `span` can accept a variety of values to set the bin size
  - E.g. continuing the same example, `span=1w` will only return a single bin because a week is greater than the returned 30-minute time range
  - E.g. continuing the same example, `span=1us` will result in an error because using a 1-microsecond bin size will result in more than 50,000 rows
    (too many)
#### others
- Fill in later if needed. There are more options, but these three are the most common I think
# Examples
## Simple aggregation function
- `sourcetype="scada" line_id="4-9" | timechart max(current)` for time range 1546300800 to 1546302600
```
---------------------------------------------
| _time               | max(current)        |
---------------------------------------------
| 2018-12-31 19:00:00 | -22.838285688402728 |
| <29 more rows>      | <29 more rows>      |
| 2018-12-31 19:15:00 | -34.18817302461874  |
| <29 more rows>      | <29 more rows>      |
---------------------------------------------
```
- For the 2 returned events, get the maximum value of the "current" field. Instead of getting the maximum value for the entire set of events, get the
  maximum value within each time bin. Since I did not specify a number of bins to use, Splunk set the maximum number of bins to be 100 and chooses
  **X** = 60. This search *is* what was used to generate the previous notes
- Return a table of statistics where the "_time" field will serve as the x-values and the "max(current)" field will server as y-values
## ?
- `sourcetype="scada" | timechart span=15m first(current) BY line_id`
```
----------------------------------------------------------
| _time               | <line 1 ID> | <line 2 ID> | etc. |
----------------------------------------------------------
| 2018-12-31 19:00:00 | -45.829...  | -162.999... | etc. |
| <95 more rows>                                         |
----------------------------------------------------------
```
- For the 864 returned events, get the first value of the "current" field for each line_id. Instead of getting the first value for the entire set of
  events (by line_id), get the first value within each 15 minute time bin.
- Return a table of statistics where the "_time" field will serve as the x-values and the "max(current)" field will server as y-values
  - The net result is exactly what I want. I originally wanted to display the raw data, and now I have. Splunk is happy to display this because the
    `first()` function ensures Splunk will only ever have one value inside of each time bin