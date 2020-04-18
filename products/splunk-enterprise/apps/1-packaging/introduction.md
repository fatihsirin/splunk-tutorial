- https://dev.splunk.com/enterprise/docs/releaseapps/packagingtoolkit
# Introduction
- 




# Inherent Packaging configuration features
- Splunk's default packaging configuration by default allows an app to conform to  
## 

# Components
- There are three key key components to understand when packaging an app: the app manifest, partitioning, and input groups
## App manifest
- 




- The app manifest stores the locations of app components and necessary dependencies
  - Dependencies allow the Packaging Toolkit to perform dependency resolution


- An app doesn't *need* an app manifest because 
## Partitioning
- Paritioning is a process where the Splunk Packaging Toolkit takes an app's source code and divides it
  - By default, the Toolkit divides an app into at least three distinct deployment packages: one for search heads, one for indexers, and one for
    forwarders
    - This is partitioning according to physical boundaries 
    - Each package only contains the necessary information, configuration, and associated code that is needed by the respective Splunk node type
- More granualar packaging according to logical boundaries requires explicit developer input
  - E.g. the Packaging Toolkit will create the three packages by default, but manual developer work is required to create different packages for
    particular forwarders
## Input groups
- Input groups group together inputs defined in `inputs.conf` with attributes, including required dependencies, included data inputs, descriptive
  information, etc.
# Including data with an app
- I don't want PNNL to have to manually upload data to their Splunk deployment to use the app
- One approach is just to create a "sample-data" directory in the app and stick some CSV files in there
## Custom index
- I need a custom index to be created when a Splunk administrator installs the app
- I don't need to worry about changing user settings to search in the new index by default. I just need to build each dashboard panel with a search
  that explicitly looks within this index
  - By default, Splunk users can search within any non-internal index
## Populate the index with data
- I need the custom index to be filled with data when a Splunk administrator installs the app
- I think I can include a monitored script as a data input with the app and write a cron job to index data from that script exactly one time 
  - I could also do something horrible like force this behavior from within the script
- 


## Thoughts ...
- Create a dummy index
  - Can the app create it's own index? This process also needs to be automatic. 
- Put David's script in the bin/ of the app
- Add the script as a data input within the app's context
- See if the script gets automatically called and Splunk shoves that data into the dummy index when the app is installed (that's what I need to happen)


# Packaging workflow
- Package the app successfully before modifying the configuration files myself because I've messed up an app by manually editing configuration files
  - After successfully packaging the app as-is, then I can modify the configuration files how I want
## Steps
1. Generate an `app.manifest` files in the root of the app directory
  - E.g. `$ slim app-manifest untopchan ./untopchan/app.manifest`
2. Augment required manifest sections if needed
3. 