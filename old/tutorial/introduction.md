- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchTutorial/WelcometotheSearchTutorial
# Introduction
- The online explanations aren't great. These notes will most certainly need to be revised
# Indexing terminology
- When the Splunk platform indexes "raw event data," it transforms the data into searchable events and stores those events in an index
- The process of transforming data so that they can later on be quickly be searched and analyzed is called indexing
    - The processed data are stored in an index as "events"
## Index
- Conceptually, an index is a repository for data
- Physically, an index is stored within flat files on an "indexer," which is a computer that is running the Splunk platform
    - Is there a 1:1 relationship between an index and a flat file? IDK
## Events
- Events are stored in the index as a group of files which fall into two categories:
    - Raw data: raw data is stored in a compressed format
    - Index files: index files include metadata that point to the raw data
- Raw data and index files are stored in sets of directories, called "buckets," that are organized by age
## Built-in indexes
- By default, all data goes into a single, preconfigured index called "main" 
    - Other built-in indexes are used for internal purposes
- Additional indexes can be created by the user
# Adding data
## Host setting
- The proper value for this setting depends on whether I'm using Splunk Cloud vs. Splunk Enterprise and whether I'm using Linux, macOS, or Windows
    - ?
# Creating new indexes
- New indexes should be created for specific purposes or specific domains of data
    - For example, new indexes should be created for different access privilages, retention periods, or logical groupings
- For optimal performance, an index should have fewer than 20 source types