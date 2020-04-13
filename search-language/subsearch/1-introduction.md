- https://docs.splunk.com/Documentation/Splunk/8.0.3/SearchTutorial/Useasubsearch - subsearch in Splunk tutorial
- https://docs.splunk.com/Documentation/Splunk/8.0.3/Search/Aboutsubsearches - subsearch documentation. Read all of the sections!
- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Format - format function
- https://answers.splunk.com/answers/7472/subsearch-fields-query-search-how-do-i-know-which-to-use.html - "query" vs "search"
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
  - Renaming one field to "search" (no two fields can share a name) will return the value of the "search" field, but only for the *first* returned
    event. Thus, only the single value contained by the first event in that field will be returned to the outer search
  - For *every* event returned by the subsearch, renaming one field to "query" (no two fields can share a name) will return the *value* of that field
    to the outer search. Thus, the value of every event in that field wil be returned to the outer search in a single logical expression
## "search" example
- The `sourcetype="oms" [search sourcetype="oms" | fields type, bus_affected | eval bus_affected="99" | rename type as search | sort _time]` command
  has a subsearch. The subsearch returns three events. If the subsearch were run as an outer search, the results would be as follows:
```
-------------------------------
| search       | bus_affected |
-------------------------------
| loss_of_gen  | 99           |
-------------------------------
| loss_of_load | 99           |
-------------------------------
| line_outage  | 99           |
-------------------------------
```
- However, renaming a field to "search" triggers special subsearch behavior. The subsearch *only* returns the value in the "search" field of the
  *first* event returned by the search
  - In this case, that value is "loss_of_gen"
- The subsearch simply returns that value alone
  - In this case, that value is `"loss_of_gen"`
- Thus, the outer search ends up as `sourcetype="oms" "loss_of_gen"`. That complete search returns the following event that contains "loss_of_gen" in
  its _raw field:
  - Notice how the value of the "bus_affected" value is irrelevent. The outer search does *not* look like `sourcetype="oms" (bus_affected=99 AND "loss_of_gen")`
  - Also observe that if I don't sort by _time, the "line_outage" value is returned by the subsearch and this changes the final result
```
----------------------------------------------------------------------------------------------------------
| started_timestamp         | restored_timestamp        | line_id | type         | bus_affected | amount |
----------------------------------------------------------------------------------------------------------
| 2019-01-01 13:45:00+00:00 | 2019-01-01 14:00:00+00:00 | 7-8     | loss_of_gen  | 7            | 1.0    |
----------------------------------------------------------------------------------------------------------
```
## "query" example
- The `sourcetype="oms" [search sourcetype="oms" | fields bus_affected | eval bus_affected='bus_affected'-1 | rename bus_affected as query]` command
  has a subsearch. The subsearch returns 3 events. If the subsearch was run as an outer search, the results would be as follows:
```
---------
| query |
---------
| NULL  |
---------
| 5     |
---------
| 6     |
---------
```
- However, renaming a field to "query" triggers special subsearch behavior. The subsearch returns every value in the "query" field (i.e. it returns
  the value of the "query' field for every returned event)
  - In this case, those values are 5 and 6
- The subsearch returns the values via a group of logical expression
  - In this case, that group is `((5) OR (6))`
- Thus, the outer search ends up as `sourcetype="oms" ((5) OR (6))`. That complete search returns the following events that contain "5" or "6" in
  their _raw field:
```
----------------------------------------------------------------------------------------------------------
| started_timestamp         | restored_timestamp        | line_id | type         | bus_affected | amount |
----------------------------------------------------------------------------------------------------------
| 2019-01-01 23:45:00+00:00 | 2019-01-02 00:00:00+00:00 | 5-6     | line_outage  | 5-6          | 1.0    |
----------------------------------------------------------------------------------------------------------
| 2019-01-01 19:15:00+00:00 | 2019-01-01 19:30:00+00:00 | 6-7     | loss_of_load | 6            | 1.0    |
----------------------------------------------------------------------------------------------------------
```
# Default subsearch behavior complex example
- `sourcetype="oms" [search type=loss_of_gen | eval type="loss_of_load"]` returns zero events while `sourcetype="oms" [search type=loss_of_gen]`
  returns one event. Why?
- The first subsearch returns an event whose "type" field is really "loss_of_gen". Then, the subsearch forces that event's "type" field to equal
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