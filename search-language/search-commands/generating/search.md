- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Search - `search` documentation
- https://answers.splunk.com/answers/216344/when-do-i-need-to-surround-a-field-name-with-singl.html - proof that the documentation on single quotes
  vs. double quotes in the context of the `search` command is garbage
# Background
- `search` is only a generating command if it is the first function in an SPL statement. If it is used later on in an SPL statement, it is a
  distributable streaming command (it filters the already returned events instead of generating new events)
- See the examples because there are a lot of fine details
# Arguments
## Required
- x
# Examples
## Never, ever use single quotes in the `search` command
- `sourcetype="scada" line_id='1-4'` returns 0 events. Why? I don't know. Quotes should be interpreted literally, so '1-4' should be equal to "1-4"
  and 1-4
- `sourcetype="scada" 'line_id'=1-4` also returns 0 events. Why? I don't know. Quotes should be interpreted literally, so 'line_id' should be equal to
  "line_id" and line_id
- The above SPL queries work fine when double quotes are used in place of single quotes or the quotes are omitted entirely
- Single quotes *are* important in `eval` expressions, where single quotes imply a field name while double quotes imply a string literal
  - This is *not* the behavior in the `search` command!!!
## Cannot compare fields directly
- `sourcetype="scada" line_id=line_id` returns zero events because there is no event whose "line_id" field contains the string literal "line_id"
- There is no way to alter this behavior. `search` always expects a "\<field>\<operator>\<value>" expression
  - Use the `where` function to compare the values of two fields directly
## The = and != operators only compare string values
- `sourcetype="scada" "Power _MVA"=195.21081151654957` returns one event as expected while `sourcetype="scada" "Power _MVA"=195.2108115165495700`
  returns zero events. Why? Because while Spunk *is* searching on the real "Power _MVA" field, it is treating every value in that field as a string.
  The second string literal has two extra 00s
- To get around this problem, see the `convert` function
## The <, >, <=, >= operators *can* compare values numerically before lexicographically
- `"Power _MVA">=180`
```
----------------------------------------------------------------------------------------------------------------------------------------
| timestamp                 | line_id | power              | origin_bus_id | origin_voltage | destination_bus_id | destination_voltage |
----------------------------------------------------------------------------------------------------------------------------------------
| 2019-01-01 21:00:00+00:00 | 1-4     | 195.21081151654957 | 1             | 1.0            | 4                  | 0.979677731806003   |
----------------------------------------------------------------------------------------------------------------------------------------
| 2019-01-01 10:00:00+00:00 | 1-4     | 189.3168494951707  | 1             | 1.0            | 4                  | 0.9831013415349404  |
----------------------------------------------------------------------------------------------------------------------------------------
```
- The search works because Splunk is treating the values in the "Power _MVA" field as numbers
## The <, >, <=, >= operators *may not* compare values numerically before lexicographically
- `sourcetype="scada" | convert num(power) | search power>="195.2108115165495700"` returns 647 events instead of the one that I wanted. Why? Because
  by enclosing the numeric literal in quotes, I have made a string literal. If these operators have a string literal on the right-hand side, they
  **will** perform lexicographic comparison
## The AND operator is always implied
- `line_id="1-4" "Power _MVA">=180`
```
----------------------------------------------------------------------------------------------------------------------------------------
| timestamp                 | line_id | power              | origin_bus_id | origin_voltage | destination_bus_id | destination_voltage |
----------------------------------------------------------------------------------------------------------------------------------------
| 2019-01-01 21:00:00+00:00 | 1-4     | 195.21081151654957 | 1             | 1.0            | 4                  | 0.979677731806003   |
----------------------------------------------------------------------------------------------------------------------------------------
| 2019-01-01 10:00:00+00:00 | 1-4     | 189.3168494951707  | 1             | 1.0            | 4                  | 0.9831013415349404  |
----------------------------------------------------------------------------------------------------------------------------------------
```
- This search is implicitly `line_id="1-4" AND "Power _MVA">=180`
## Search the _raw field of an event instead of a specific field
- The SPL `1` returns 395 events because Splunk searches the _raw field of all events for *individual word instances* of the character "1"
  - E.g. ",1-4" is a match because "1" is surrounded by two non-alphanumeric characters
  - E.g. "2019-" is not a match because "1" is part of the larger word "2019"
  - If `*1*` is searched, then 867 events are returned because every SCADA event and every OMS event has a _raw field that contains the character "1"
    somewhere
- The _raw field contains the raw data of the event in text format
- This behavior shows that Splunk searches on the _raw field by default when no other field is specified
## Use the IN operator for lengthy comparisons
- `line_id IN (1-4, 4-9)` returns 192 events whose "line_id" field is either "1-4" or 4-9"
  - This is more convenient than writing `line_id=1-4 OR line_id=4-9`