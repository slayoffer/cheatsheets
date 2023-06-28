### Add source

```bash
nuget sources add -Name "telerik.com" -Source "https://nuget.telerik.com/v3/index.json" -UserName "info@rubius-soft.com" -Password "B271m8Gz9m4eMT"
```

### Pull

```bash
nuget install Telerik.WebReportDesigner.Services -Version 17.0.23.315 -Source "telerik.com"
# or
nuget install Telerik.WebReportDesigner.Services -Version 17.0.23.315 -Source https://nexus.rubius.com/repository/telerik-nuget-proxy/index.json
```
