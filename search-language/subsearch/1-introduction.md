- https://docs.splunk.com/Documentation/Splunk/8.0.3/SearchTutorial/Useasubsearch - subsearch in Splunk tutorial
- https://docs.splunk.com/Documentation/Splunk/8.0.3/Search/Aboutsubsearches - subsearch documentation. Read all of the sections!
- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Format - format function
# Default subsearch behavior
- By default, a subsearch returns its results to the outer search as a set of grouped logical expressions
  - Each individual event is returned as group of logical expressions within a single pair of parentheses
  - If the subsearch returns multiple events, each event would be within its own set of parentheses and each set of parentheses would be linked by an
    `OR` operator
- A subsearch has this behavior because its results are always implicitly passed to the `format` command
## Example
- The `sourcetype="oms" [search type=loss_of_load]` command has a subsearch. The subsearch returns one event. The results of that subsearch are
  returned to the outer query as a group of logical expressions, where each individual expression is a key-value `search` expression:
```
(started_timestamp="2019-01-01 19:15:00+00:00" AND restored_timestamp="2019-01-01 19:30:00+00:00" AND type=loss_of_load AND etc...)
```
- As a result, the final outer query looks like this (remember that `AND` implicitly joins `search` expressions):
```
sourcetype="oms" (started_timestamp="2019-01-01 19:15:00+00:00" AND restored_timestamp="2019-01-01 19:30:00+00:00" AND type=loss_of_load) AND etc...)
```
- The outer search only finds one event that meets all of the search expression criteria and it returns that event
  - It just so happens that the event returned by the outer search is the same event that was returned by the subsearch
# Special subsearch behavior
  - Renaming a field to either "search" or "query" triggers special behavior for a subsearch
## Example
- The `sourcetype="oms" [search type=line_outage | eval type="of" | rename type as search]` command has a subsearch. The subsearch returns one event.
  You would think that the results of that subsearch would be returned in the same way as the first example as follows:
  - Note how the "type" field was renamed to "search" and made to contain the value "of". Renaming the "type" field to "search" makes the subsearch
    return *only* the **field value** to the outer query (it returned "of" instead of "search=of")
```
(started_timestamp="2019-01-01 19:15:00+00:00" AND restored_timestamp="2019-01-01 19:30:00+00:00" AND "of" AND etc...)
```
- However, this is **not** the case. I make this claim based on the actual two events returned by the full search. Neither of the two events matches
  the *complete* above group of logical expressions, just the "of" string expression
- In reality, the final outer query looks like this:
  - The special behavior of the subsearch trigerred this behavior
```
sourcetype="oms" "of"
```
- The outer search returns two events that have the "of" word within their _raw field
# Default subsearch behavior complex example
- `sourcetype="oms" [search type=loss_of_gen | eval type="loss_of_load"]` returns 0 events while `sourcetype="oms" [search type=loss_of_gen]` returns
  one event. Why?
- The first subsearch gets an actual event who's "type" field is really "loss_of_gen". Then, the subsearch forces that event's "type" field to equal
  "loss_of_load". When the subsearch returns that weird event to the outer search, the outer search finds no matching events? Why? For starters, there
  is no event that meets the following criteria: `(restored_timestamp="2019-01-01 14:00:00+00:00" AND type="loss_of_load")`. The other criteria also
  wouldn't match
- The second subsearch gets the same actual event, but doesn't modify it. As a result, when that event is returned to the outer search, a single event
  can be found by the outer search that matches *all* of the logical expressions returned by the subsearch.
# Troubleshooting
## Numeric vs. string fields
- Why doesn't this work?: `sourcetype="scada" [search sourcetype="scada" | stats max(power) as power]`
  - It's because this also doesn't work: `sourcetype="scada" power=195.2108115165495700`
    - Why doesn't that work? Because the `search` command always does string comparisons with the = operator 
      -  The value "195.2108115165495700" is returned by `stats max`. I assume that `stats max` returns a true numeric result. That's great, except
         for the fact that the real value in the "power" field of the matching event is "195.21081151654957" (notice the two missing 0s). As strings,
         "195.21081151654957" != "195.2108115165495700"
### Solution
- See the `convert` command