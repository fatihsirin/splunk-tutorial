- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/SearchTimeModifiers - detailed breakdown of specifying time ranges in the search
  bar
- https://docs.splunk.com/Documentation/Splunk/latest/Search/Specifytimemodifiersinyoursearch - less detailed breakdown of the same content
# How to specify a time bound
## Via the time range picker
- Most of the time, I'll use the time range picker to the right of the search bar to apply a time bound to the base search and all subsearches
## Via time modifiers in the search bar
- It is also possible to specify a time bound directly in the search bar with "time modifiers"
- The `earliest` time modifier defines an _inclusive_ lower bound
- The `latest` time modifier defines an _exclusive_ upper bound 
- The inclusive/exclusive nature of these bounds is consistent in the time range picker and with time modifiers
# Questionable syntax
- I don't understand the `earliest >= <time>` or `latest < <time>`, etc. syntax
  - Using the > or < symbols doesn't appear to affect the results differently from using =
# Additonal fun time functionality
## Create custom present time ranges
- It is possible to create my own custom preset time ranges in the Splunk time range picker
## Change the default time range picker value
- The default time range picker value is the last 24 hours. This can be changed to whatever I want

I hate the way these notes look but I don't know how to make them more appealing