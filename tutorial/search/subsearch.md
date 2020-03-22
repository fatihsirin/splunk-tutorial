- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchTutorial/Useasubsearch
# Usage
- Subsearches are useful because they allow a single search to return the same result that could normally only be created by two or more searches
- A subsearch is processed before any outer searches
# Syntax
- A subsearch is enclosed in square brackets
    - E.g.
    ```
    sourcetype=access_* status=200 action=purchase [search sourcetype=access_* status=200 action=purchase | top limit=1 clientip | table
    clientip] | stats count, distinct_count(productId), values(productId) by clientip
    ```
    - This subsearch returns the IP of the most active client on the website
        - Actually, it must return something like `clientip=<IP>`, not just `<IP>`, because that's the only way the syntax makes sense
## Pretty formatting
- `Command + \` will apply pretty formatting to the search text itself
- Use aliases to rename columns if desired
# Performance
- The performance of this subsearch depends on how many distinct IP addresses match `status=200 AND action=purchase` for the subsearch
    - Those same search terms are used in the outer search, but the above point is specifically about the subsearch
- By default, subsearches return a maximum of 10,000 results and have a maximum run-time of 60 seconds
    - To get around this, rewrite the query to be more selective or increase the max results and max run-time parameters