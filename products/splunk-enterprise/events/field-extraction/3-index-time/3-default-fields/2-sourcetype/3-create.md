# When to create a new soucetype
## Knowledge objects
- A sourcetype is used to group associated knowledge objects together
### Field extractions
- When I create a bunch of field extractions on a particular sourcetype, those field extractions will apply to all data that is classified as that
  soucetype
  - E.g. if I create SCADA field extractions for the "csv" soucetype, then *any* csv will have those field extractions applied
  - This is sometimes not what I want. Now I see that I want to create a new soucetype when I want to group knowledge objects (e.g. field extractions)
    under that soucetype
    - E.g. create a "SCADA-csv" soucetype and make it inherit from the "csv" soucetype