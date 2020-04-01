- https://docs.splunk.com/Documentation/Splunk/8.0.2/AdvancedDev/ModInputsBasicExample - basic example
# Execution modes
- Every modular input must have an underlying script
    - The script can be written in Java, C#, Python, or JavaScript
- Typically, the script runs in three modes:
    - introspection (required)
    - execution (required)
    - validation (optional)
## Introspection mode
- Introspection mode defines the endpoints and behavior of the script
- The script must have an introspection routine, and must define the `--scheme` command-line argument that will cause the routine to be
  invoked
    - The term "routine" can be used interchangably with the term "function," but originally "routine" describes a block of reusable logic that
      *doesn't return anything*
### Example
```
# Empty introspection routine
def do_scheme(): 
    pass
```
## Execution mode
- Execution mode streams data for indexing
- This is the mode that runs when the script is invoked without any command-line arguments (e.g. `$ python my_script.py`)
## Validation mode
- Validate mode validates input data to ensure the script only accepts valid data
- Define the `--validate-arguments` command-line argument and use it to invoke the validation routine