- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Lookup
# Examples
## Lookup latitude and longitude based on "Bus Affected"
### Search
```
`sourcetype="oms" | lookup bus-locations-lookup-def bus_id AS "Bus Affected" OUTPUTNEW latitude as lat longitude as lon`
```
```
---------------------------------------------------------------------------------------------------------------------------------------
| Event Datetime            | Restoration Datetime      | Line ID | Type         | Bus Affected | Amount _MW | lat       | lon        |
---------------------------------------------------------------------------------------------------------------------------------------
| 2019-01-01 23:45:00+00:00 | 2019-01-02 00:00:00+00:00 | 5-6     | line_outage  | 5-6          | 1.0        | NULL      | NULL       |
---------------------------------------------------------------------------------------------------------------------------------------
| 2019-01-01 19:15:00+00:00 | 2019-01-01 19:30:00+00:00 | 6-7     | loss_of_load | 6            | 1.0        | 38.994154 | -77.040912 |
---------------------------------------------------------------------------------------------------------------------------------------
| 2019-01-01 13:45:00+00:00 | 2019-01-01 14:00:00+00:00 | 7-8     | loss_of_gen  | 7            | 1.0        | 38.975811 | -77.024985 |
---------------------------------------------------------------------------------------------------------------------------------------
```
- For the three returned events, attempt to use the "bus-locations-lookup-def" lookup definition (which must already exist). Use the lookup definition
  to create a mapping between the "bus_id" lookup field and the "Bus Affected" event field. If the mapping can successfully be evaluated (e.g. the
  "bus_id" column of the lookup table file has a row with a value of "6" and the event also has a value of "6" for the associated "Bus Affected"
  field), output the additional data from the lookup table file
  - In this case, that addition data is contained in the "latitude" and "longitude" columns of the lookup table file
- Return the three modified events with additional data supplied by the lookup table + lookup definition
## Lookups field values must match exactly!
### Search
```
`untopchan_index_macro` sourcetype=realtime_scada | where Line_ID="1-4" | sort _time | rex field=Line_ID "(?<r_fbus>\d)-(?<r_tbus>\d)" | lookup line-ratings r_fbus OUTPUT r_rateA
```
- This lookup doesn't work because the "r_fbus" field in my events is "1", but the "r_fbus" field in the lookup file is "1.0". ARGH! 
  - What this means is that lookups are always done based on text matching, not numeric values
# Purpose
- The `lookup` command is used to manually use an existing lookup definition
- The command becomes an orchestrating command when `local=true`, which forces the `lookup` command to run only on the search head
  - By default, `local=false`
# Arguments
- `... | lookup <lookup definition> <lookup field 1> <lookup field 2> ...`
## Required
- The name of a single lookup definition
  - E.g. "bus-locations-lookup-def"
## Optional
- One or more \<lookup fields>
- The documentation says that it is optional to associate an event field with a lookup table heading
  - E.g. `bus_id AS "Bus Affected"` could simply be written as `bus_id`
  - However, I think this is only makes sense if the events contain a field with the exact same name as the desired lookup table heading
    - E.g. pretend that the "Bus Affected" field is called the "bus_id" field within the events themselves
    - Yes, this is true as evidenced by the fact that this command works as expected:
      `sourcetype="oms" | rename "Bus Affected" as bus_id | lookup bus-locations-lookup-def bus_id OUTPUTNEW latitude as lat longitude as lon`
- A field output directive
  - By default, *all* of the lookup fields that weren't used as match fields are output as new fields for the events. Specifying an output directive
    only outputs the *specified* lookup fields for the events
    - `OUTPUT`: this directive specifies which fields to output *and* if a lookup field name matches an existing event field name, original event value
      will be overwritten
    - `OUTPUTNEW`: this directive specifies which fields to output *and* if a lookup field name matches an existing event field name, the original
      event value is preserved