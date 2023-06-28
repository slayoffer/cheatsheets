### Check

```bash
# list package by using Nexus proxy
nuget list packageid:newtonsoft.json -Source https://nexus.rubius.com/repository/nuget.org-proxy/index.json
```

### Install package

```bash
# install via group
nuget install System.Text.Json -Source https://nexus.rubius.com/repository/nuget-group/index.json # DONT FORGET /index.json !!!

# install via proxy
nuget install newtonsoft.json -Source https://nexus.rubius.com/repository/nuget.org-proxy/index.json

# install with version num.
nuget install Telerik.WebReportDesigner.Services -Version 17.0.23.315 -Source https://nexus.rubius.com/repository/telerik-nuget-proxy/index.json
```

### Push package

```bash
# to hosted registry
nuget push ~/.nuget/packages/newtonsoft.json/13.0.3/newtonsoft.json.13.0.3.nupkg -Source https://nexus.rubius.com/repository/nuget-hosted/ -ApiKey d2c67245-2bfb-3f23-8a7c-b6e3d797ee34
```

### Add group

```bash
nuget source add -name rubius-nuget-group -source https://nexus.rubius.com/repository/nuget-hosted/index.json
```

### List packages in repo

```bash
nuget list -Source https://nexus.rubius.com/repository/nuget-hosted/
```

### List sources

```bash
nuget sources list
```

### Cache

```bash
# show
nuget locals all -list

# clean
nuget locals all -clear
```

