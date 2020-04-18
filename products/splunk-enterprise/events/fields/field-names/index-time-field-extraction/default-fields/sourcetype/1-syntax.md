- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Configurationfilestructureandsyntax
# Case sensitivity
- In a configuration file, attributes of a \<spec> are case-sensitive, but that doesn't answer the question about sourcetypes, which are a type of \<spec>
```
[<spec>]
IamACaseSensItiVeAttRiButE = <value>
```
- A \<spec>, such as a sourcetype, is case-sensitive in an `eval` expression
  - It is more accurate to say that matching against a double-quoted sting literal in an `eval` expression takes case into account
- The only place where a \<spec> is matched case-insensitively seems to be within the `search` command
- Therefore, the answer is yes, \<specs> are case-sensitive, it's just that `search` has different behavior
## Within a `search` context
- A \<spec> is case-insensitive within the `search` command
  - E.g. `[circuitComponent]` can be searched as `sourcetype=cirCuitCompOnent`
## Within an `eval` expression
- A \<spec> is _case-sensitive_ within an `eval` expression
  - E.g. `index=main | where sourcetype="circuitcomponent"` does not work but `index=main | where sourcetype="circuitComponent"` does