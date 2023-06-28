## Convert a CSV file to Json file

You need to have python installed.

```bash
cat test.csv | python3 -c 'import csv, json, sys; print(json.dumps([dict(r) for r in csv.DictReader(sys.stdin)]))'
```



