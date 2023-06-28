## Simulate the execution of some commands with echo

For example, in this case, we see which files with the .csv extension will be deleted.

```
echo rm *.csv
```

### Append to the end of file

```bash
echo "This text will be appended to that file" >> testfile.txt
# will not replace original content of the file

# or
echo "This text will be appended to that file" | tee -a testfile.txt # -a for append
```