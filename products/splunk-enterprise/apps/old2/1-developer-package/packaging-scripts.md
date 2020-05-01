# Example
- x
# Purpose
- A data source script can be bundled with an app to serve as a surrogate data input
# Data source script dependency installation
- The tricky thing about packing a data source script with an app is that the script must execute in the production environment
  - Therefore, all of the necessary dependencies must be in place in the production environment
- The best approach is to write an installation script for the data source script:
  - The correct version of Python must be installed
  - Any Python dependencies must be installed
    - E.g. oct2py, dateutil
  - Any non-Python dependencies must be installed
    - E.g. octave, matpower
  - The data input script must not mess up due to Python pathing
- If any dependency cannot be installed on the development environment, then the script shouldn't be packaged with the app
  - E.g. matpower requires octave 4.0.0 or above, but yum only ships with octave 3.8.2. We can't build octave 4.0.0 from source because yum only has
    automake 1.13.4 while building octave requires 1.14
  - In the future, it would be good to explicitly examine requires for non-Python dependencies because those are the trickiest!
