# Sed is needed to filter and modify text without editors

### Fine and replace text

```bash
# replace Pineapple with Feta in text.txt file
sed 's/Pineapple/Feta/' text.txt # this cmd wont overwrite the file
# to overwrite file
sed -i 's/Pineapple/Feta/' text.txt # -i for change in place

# example
sudo sed -i 's/index.html/index.php/g' /etc/httpd/conf/httpd.conf

# will also work with other delimetres
sed 's|Pineapple|Feta|' text.txt
sed 's Pineapple Feta ' text.txt
sed 's.Pineapple.Feta.' text.txt
```

### Remove some string

```bash
sed 's/Pineapple//' text.txt # remove Pineapple and replace with '' (delete)

# better use . or | instead of /
sed 's./etc..' paths.txt # replace /etc with '' (delete)
```

### Piping example

```bash
echo 'hello' | sed 's/hello/goodbye/' # will show 'goodbye'
```

### Args

```bash
:[label] Set label
Zero/One address commands
= Print current line number
a \ Append text
text (embedded newlines)
Address range commands
b [label] Jump to label
t (T) [label] Jump to label on (failed
c \ Replace match
text (Embedded newlines)
d (D) Delete (to newline)
h H Copy/Append pattern
g G Paste/Append hold
x Exchange hold/pattern
n N Read/Append next line
p Print pattern
s/regex/replace/ Substitution

# Addresses
first~step Starting at first, every step
/regex/ Lines matching regex
addr1,+N addr1 and N following

# Various
{c1;c2;c3;} List of commands
; Separate commands
```

