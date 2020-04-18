- https://docs.splunk.com/Documentation/Splunk/8.0.3/Search/Aboutrealtimesearches
# Real-time search vs. real-time indexing
- Splunk starts indexing raw data into events as soon as that raw data arrives at an indexer. Thus, there is no real-time distinction for indexing.
  Splunk is already indexing as fast as it can
- However, there _is_ a distinction between historical and real-time searches
- Real-time searching allows a user to search events _before_ they are indexed and preview reports as events stream in
  - The matching events within a time window (e.g. 1-minute since now) are displayed and older events are removed as they expire
# Indexed real-time search
- There are also "indexed real-time searches", which consume less resources than regular real-time searches
- These behave just like historical searches, except that the search results are continually updated as events are written to disk
- Use indexed real-time search when up-to-the-second accuracy is not needed
- The sync delay of an indexed real-time search can be adjusted
  - The shorter the delay, the greater the probability of an event being "missed" and not displayed in the results because ...