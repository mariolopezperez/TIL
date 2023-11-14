### Enumerating Windows services along with its startup time

```
foreach($Service in (Get-Service)) {
     $startup_type = sc.exe qc "$($Service.ServiceName)" | Select-String "START_TYPE" | ForEach-Object { ($_ -replace '\s+', ' ').trim().Split(" ") | Select-Object -Last 1 }
     Write-Host "$($Service.ServiceName);$($Service.DisplayName);$startup_type"
}
```
