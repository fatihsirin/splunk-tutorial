- https://dev.splunk.com/enterprise/docs/releaseapps/packagingtoolkit#The-app-manifest - `app.manifest` documentation
# Example
## File
### X
```
<beautiful app.manifest>
```
# Purpose
- The purpose of `app.manifest` is to store information that, later on, will inform the `$ slim partition <app.tar.gz>` command how to partition the
  app source code into sub-packages
  - This contrasts with `app.conf`, which stores configuration information about the app itself
# CLI commands
- `$ slim generate‑manifest <app root dir> [-o <output file>]`
  - E.g. `$ slim generate-manifest untopchan -o untopchan/app.manifest`
- The contents of the file will be populated from the contents of `app.conf`
# Editing workflow
- First, generate the manifest with the CLI, then fill in the extra sections as needed
  - If I don't need to fill in the extra sections, then I never _needed_ to generate an `app.manifest` in the first place
# Files
- An explicit `app.manifest` doesn't need to exist, since Splunk can infer its contents when performing packaging, but I should have one because:
  - It's good practice 
  - Advanced configuration beyond the three generated sub-packages requires an explicit `app.manifest`
- If it exists, `app.manifest` must be located at the root of the app directory