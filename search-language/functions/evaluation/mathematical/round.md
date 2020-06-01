# Examples
## Round field to tenth's place
### Search
```
| makeresults annotate=true | eval my_value=1.327 | eval my_value=round(my_value, 2) | eval other_value=my_value*2
```
```
--------------------------------------------------------------------------------------------------------------------------------------------------------
| _raw | _time               | host | my_value | other_value | source | sourcetype | splunk_server                               | splunk_server_group |
|      | 2020-05-26 21:15:35 |      | 1.33     | 2.66        |        |            | server-Austins-MacBook-Air.local-SearchHead |                     |
--------------------------------------------------------------------------------------------------------------------------------------------------------
```
- `round` rounds 1.327 to 1.33
## Format field to tenth's place
```
| makeresults annotate=true | eval my_value=1.327 | fieldformat my_value=round(my_value, 2) | eval other_value=my_value*2
```
```
--------------------------------------------------------------------------------------------------------------------------------------------------------
| _raw | _time               | host | my_value | other_value | source | sourcetype | splunk_server                               | splunk_server_group |
|      | 2020-05-26 21:15:35 |      | 1.33     | 2.654       |        |            | server-Austins-MacBook-Air.local-SearchHead |                     |
--------------------------------------------------------------------------------------------------------------------------------------------------------
```
- `fieldformat` is like `eval`, except that the underlying data isn't modified, it's just displayed differently
# Purpose
- `round` is the easiest way to truncate floating-point numbers