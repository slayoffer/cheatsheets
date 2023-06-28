### Bat Install

```bash
# ubuntu
sudo apt install bat

# fedora
sudo dnf install bat

# CentOS
sudo dnf instal tar
```

### Use

```bash
# ubuntu / debian
batcat file.txt
# better add alias to .bashrc
alias bat="batcat"
exec bash # to reload terminal without actual reload

# for fedora
cat file.txt
```

### Cat mode

```bash
batcat --paging=never file.txt
```

### Show all chars (whitespaces, tabs, spaces, etc)

```bash
batcat --show-all file.txt
```

### Show git changes in file

```bash
# no args needed
```

### Cat command

```bash
cat text.txt # cat stands for concatenate

# cat several files
cat text.txt text2.txt

# create new file with content of old file
cat text.txt > text2.txt

# cat with line numbers
cat -n text.txt # -n for numbers

# find "cars" strings in file
cat text.txt | grep cars

# find with color
cat text.txt | grep --color cars

# show with lines number
cat text.txt | wc -l # -l for lines
```

