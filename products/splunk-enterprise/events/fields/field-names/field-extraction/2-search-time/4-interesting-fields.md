- https://docs.splunk.com/Documentation/Splunk/8.0.2/Search/WhatsinSplunkSearch - first mention of interesting fields
- https://answers.splunk.com/answers/474913/how-to-map-certain-extracted-interesting-fields-to.html - confirmation that I can create field aliases on
  interesting fields
# What are interesting fields?
- Interesting fields are those fields that appear in at least 20% of the results returned by a search
- Splunk doesn't say much about them. I'll think of them as automated field extractions that I didn't have to configure myself. Since they are real,
  bona fide field extractions, I can create field aliases that link to them if i want