- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Overviewofeventprocessing - event processing overview
# Event processing is huge
- Breaking down the order of event processing will help me understand this complex process
# Order of event processing
1. Configure the character set encoding
2. Configure line-breaking for multi-line events
3. Extract event timestamps
4. Extract index-time default fields
    - Creating a sourcetype is not part of event processing. Using a sourcetype is. Therefore, notes on creating sourcetypes should not go in this area
5. Segment events
6. (Optional) Dynamically assign metadata to events
7. (Optional) Anonymize data