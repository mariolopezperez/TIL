### Print disabled AD users based on an input list CSV

Useful to correlate license usage on a SaaS solution.

`
Import-csv -Path .\use_export.csv -delimiter "," | ForEach {
  Get-ADUser -Filter "EmailAddress -eq '$($_.User)'" -Properties EmailAddress | Where { $_.Enabled -eq $True} | Select Name,samaccountname,EmailAddress
}
`
