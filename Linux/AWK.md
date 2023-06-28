### Print contents of file

```bash
awk '{print}' text.txt # alternative to cat cmd
# or
awk '{print $0}' text.txt

# show first column for every line
awk '{print $1}' text.txt

# show first field for every line separated by ':'
awk -F':' '{print $1}' /etc/passwd
# -F':' is to separate by every found ":"
# '{print $1}' is to include first word
```
### Print more than one field

```bash
awk '{print $1,$3}' text.txt # print words 1 & 3
```

### Specific field number

```bash
# print last column
awk '{print $NF}' text.txt # NF last number of fields
```

### Show which shells are being used by users

```bash
awk -F':' '{print $1,#7}' /etc/passwd
```



