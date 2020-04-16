- https://answers.splunk.com/answers/210931/what-are-the-pros-and-cons-between-using-apps-vers.html - purpose of creating an app
# What is an app?
- An app is a way to easily package and distribute Splunk configurations in an organized way
    - E.g. field extractions, views, lookups, custom commands, etc.
    - Some apps expect to find data in specific indexes for performance reasons, while others don't
# What is the relationship between an app and an index?
- Separate indexes outside of `main` provide ways to:
    - Apply different data access and retention policies to different types of data
    - Optimize performance
    - etc.
- Thus, there is no clear relationship between the concept of an app and an index. An app could use the `main` index, or it could use custom indexes.
  There is no standard answer to the question
# Purpose of creating a custom app
- What is the point of creating a custom app, as opposed to just using the generic "Searching & Reporting" app?
    - All "apps," including the generic "Searching & Reporting" app all do the same thing: they let you search data
- It seems to me that the "Searching & Reporting" app is just a generic app that exists because it is easy for new users to understand its purpose
    - A user understands that they should click on the "Searching & Reporting" app to start searching stuff
- If all apps exist to let you search data, then what is the value of creating a custom app?
- The purpose is to easily package and distribute particular Splunk configurations in an organized way
    - What if I wanted to install 10 different apps? It would be crazy to expect all of their configurations to live inside of the search app
