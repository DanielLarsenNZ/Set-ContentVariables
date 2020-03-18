# Set-ContentVariables

Similar to `envsubst` but for PowerShell. Replace placeholders in a file with values from input environment 
variables and/or input variables.

## Usage

```powershell
. ./Set-ContentVariables.ps1

Set-ContentVariables -Path './settings.ini' -Variables @{ setting1 = 'settingvalue1'; }
```

## Placeholder formats

* `${VAR_NAME}`
* `$(VAR_NAME)`

## Example

Replaces any occurrance in `test.json` of `${myVar1}` and `${myVar2}` with the matching value in the
`-Variables` parameter object. Also replaces any occurrance of a placeholder named for an environment 
variable with the value of that variable, e.g. `$(COMPUTERNAME)`.

Given this file `test.json`:

```json
{
    "property1": "${myVar1}",
    "property2": "${myVar2}",
    "computerName": "${COMPUTERNAME}"
}
```

Run this PowerShell:

```powershell
. ./Set-ContentVariables.ps1

Set-ContentVariables -Path './test.json' -Variables @{ myVar1 = 'value1'; myVar2 = 'value2'}
```

`test.json` will be rewritten to:

```json
{
    "property1": "value1",
    "property2": "value2",
    "computerName": "DANSLAPTOP"
}
```

## Parameters

* **Path** - Path to the file that will have its values replaces
* **Variables** - Optional. A PSObject of variables to find and replace. e.g.<br/> 
  `@{ firstVar = 'first value'; secondVar = 'second value' }`

## Notes

* Variables that cannot be found are ignored.
* `Variables` parameter variable overrides an environment variable of the same name.
