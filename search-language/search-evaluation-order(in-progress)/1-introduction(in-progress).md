- https://docs.splunk.com/Documentation/SplunkCloud/latest/Knowledge/Searchtimeoperationssequence - order of search time operations
# Search workflow order summary
1. Inline field extractions
    - These are `EXTRACT-<class>` attributes in `props.conf` stanzas
    - Fields that are extracted by an inline field extraction whose name has an earlier lexicographical sort position _can_ be referenced inside
      inline field extractions having a name with a later lexicographical sort position
      - E.g. within the _same_ stanza, fields extracted by `EXTRACT-aaa` can be referenced _within_ `EXTRACT-zzz`, but not vice versa
2. Transform field extractions
    - These are `REPORT-<class>` attributes in `props.conf` stanzas, along with stanzas in `transforms.conf`
3. Automatic key-value extractions
    - These are `KV_MODE` attributes in `props.conf` stanzas
4. Field aliasing
    - These are `FIELDALIAS-<class>` attributes in `props.conf` stanzas
5. Calculated fields
    - These are `EVAL-<fieldname>` attributes in `props.conf` stanzas
      - Notice how \<fieldname> is used instead of \<class>. This is because \<class> is the name of a field extraction while \<fieldname> is the
        _actual_ name of the derived field
    - Remember that calculated fields within the same `props.conf` stanza cannot reference each other
      - Thus, it makes no sense to consider the lexicographical ordering of calculated fields
6. Lookups
    - These are `LOOKUP-<class>` attributes in `props.conf` stanzas, along with stanzas in `transforms.conf`
7. Event types
    - These are stanzas within `eventtypes.conf`
8. Tags
    - These are stanzas within `tags.conf`
# Lexicographical ordering

I AM CONFUSED ABOUT THE SEARCH WORKFLOW ORDER. ARE ALL INLINE FIELD EXTRACTIONS FOR ALL STANZAS PROCESSED FIRST? OR IS EACH STANZA PROCESSED SEQUENTIALLY???

- Within a \<spec> stanza, Splunk processes the following "knowledge objects" within that stanza according to lexicographical order:
  - Inline field extractions
  - Transform field extractions
  - Field aliases
  - Event types (after accounting for priority)
  - Lookups
- In other words, Splunk goes through and examines each stanza separately. 


- The configurations themselves are not ordered. Remember that each of these configurations exists within a \<spec> defintion (i.e. a host, source, or
  source type stanza). Conceptually, Splunk gathers all the \<spec> stanzas and alphabetizes them. Then, it goes through each stanza in order and
  evaluates all inline field extractions. Then it goes though each stanza and evaluates all transform field extractions, etc.
  - Splunk also processes tags in lexicographical order, but their stage is last _and_ they aren't associated with any \<spec>











- https://docs.splunk.com/Documentation/Splunk/6.1.4/Search/Aboutsearchlanguagesyntax#Fields - all fields are strings
# Splunk fields only contain strings
- Each value of a field is a text string
  - Numbers are *strings* that only contain numeric literals
    - E.g. the field containing the value of the number 10 contains the characters "1" and "0"
  - Commands that take numbers from string values automatically convert the values *internally* to numbers for calculations
## Repercussions
### Searching for numeric values is difficult
- Since all Splunk field values are strings, searching for events that contain a specific numeric values is extremely tricky
  - E.g. `sourcetype="scada" "Power _MVA"=195.21081151654957` returns one event as expected while `sourcetype="scada" "Power
    _MVA"=195.2108115165495700` returns zero events. Why? Because while Spunk *is* searching on the real "Power _MVA" field, the `search` command only
    does string comparisons with the = operator, so only the first query exactly matches the string value stored within an event. The second query has
    two extra 0s
#### Solutions
- See the `convert` function
- Using a `trim` function is not the solution because I never know how a numeric value will be stored in _raw