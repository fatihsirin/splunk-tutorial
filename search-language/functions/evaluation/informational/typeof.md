- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/InformationalFunctions#typeof.28X.29
# Examples
## Get type of field
### Search
```
`untopchan_index_macro` sourcetype=realtime_scada
| where Line_ID="4-5"
| rex field=Line_ID "(?<r_fbus>\d)-(?<r_tbus>\d)"
| eval r_fbus='r_fbus'+".0", r_tbus='r_tbus'+".0"
| lookup line-ratings r_fbus OUTPUT r_rateA
| eval r_rateA_Type=typeof(r_rateA)
```
- I am surprised that "r_rateA_Type" == "multivalue". This explains why this search wasn't functioning as expected
  - It was because I was using a lookup only based on "r_fbus" when I needed to also use "r_tbus"
# Purpose
- `typeof` returns the type of the specified field
  - It returns "Invalid" if the field is is analyzing is NULL (a.k.a. empty)