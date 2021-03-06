Tips
------
### script input
Use `param ( )` instead of `Read-Host` in your script.
```PowerShell
param (
    [Parameter(Mandatory = $true)]
    [string]$str
)
$str
```
and `addCommand()` with params array in your node app.

### parse output
Use [PSObject](https://devopscollective.gitbooks.io/the-big-book-of-powershell-gotchas/content/manuscript/new-object_psobject_vs_pscustomobject.html) to represent the output of your commands,
then pipe it to `| ConvertTo-Json -Compress` command.
```PowerShell
$obj = [PSCustomObject]@{name="obj"; value="output"}
$obj | ConvertTo-Json -Compress
```
now you can use `JSON.parse()` to parse the output to a JS object.

### use pipeline
Use `addCommand()` with the `@param` syntax.
```javascript
ps.addCommand('Get-Process @ComputerName | where {$_.ProcessName -eq "powershell"}', [
  {ComputerName: 'localhost'}
]);
```

### cast JS to PS data types
Node-PowerShell can help you convert JS data types to PS data types.
All you need to do is to send the parameters to your script as follows:
```javascript
{string: 'abc'} <-> [string]$string
{char: 'a'} <-> [char] $char
{byte: '0x26'} <-> [byte] $byte
{int: 1} <-> [int]$int
{long: 10000000000} <-> [long]$long
{bool: true} <-> [bool]$bool
{decimal: '1d'} <-> [decimal]$decimal
{double: 1.1} <-> [double]$double
{DateTime: new Date()} <-> [DateTime]$DateTime
{xml: '<a></a>'} <-> [xml]$xml
{array: [1,2]} <-> [array]$array
{hashtable: {A:1, B:2}} <-> [hashtable]$hashtable
```
