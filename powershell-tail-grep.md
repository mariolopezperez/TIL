### Powershell Tail + Grep

```
Get-Content .\localhost_access.log -tail 10 -wait | select-content "no such user"
````
