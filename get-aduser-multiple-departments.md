### Get AD users that belong to list of departments and are enabled

Get AD users filtering which belong departments which satisfy the following: IT, DEV*, FINANCE or HR.*, which are enabled. Save the results to users.txt

````
Get-ADUser -Filter {(
(Department -Like 'IT') -or 
(Department -Like 'DEV*') -or 
(Department -Like 'FINANCE') -or 
(Department -Like 'HR.*')) -and (Enabled -eq "True")}  -properties SamAccountName | select @{Label = 'SamAccountName' ; Expression = {$_.SamAccountName.ToLower()}} | Out-File -FilePath users.txt
```
