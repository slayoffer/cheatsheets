### Extract value

```json
$.property1
```

### Any value query

```json
@

# find smth
$.property1.[1].[?(@.location == "Malala")].model
$.property1.[1].[?(@ != Malala)]
$.property1.[1].[?(@ > 40)]
$.property1.[1].[?(@ in [40,40,41])]
$.property1.[1].[?(@ nin [40,40,41])]
```

