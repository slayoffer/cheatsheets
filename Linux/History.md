### Show CMDs without numbers

```bash
history -w /dev/stdout | sed 's/^[ 0-9]*//' | less
```



### Поиск команд по истории

```bash
# Вывести все команды с нумерацией
history

# Show only several lines
history 4

# Поиск по истории через ключевое слово
history | grep install

# Запустить команду из списка по ее номеру
!156

#/ Вывести команду из списка, но не запускать
!156:p (print)

# Вывести первую/пятую команду с конца
!-1 / !-5

# Вывести прошлую команду, содержащую аргумент или слово
!find / !sudo / !cat

# Подставить все аргументы из прошлой команды кроме первого
mkdir data
cd !* # cd data

cp text.txt text2.txt
!* # text.txt text2.txt

# Подставить аргумент из прошлой команды по его номеру
cp file.txt file2.txt
cat !:2 # cat file2.txt

ls -l
!:0 # ls

# Подставить диапазон аргументов
ls -a -f -h -F -m
ls !:2-4 # ls -f -h -F

# Подставить последний/первый аргумент из команды
ls !$ / ls !^

ls !:2-& (подставить все аргументы со второго и до конца)

ALT + . (через сочетание клавиш. Если нет аргументов, то подставится сама команда)
```

### Delete from history

```bash
history -d 566
```

### History file

```bah
cat /.bash_history
```

### Change long cmd contents

```bash
# tab to cmd and press
Ctrl + x + e 
# will open editor for cmd and exec new cmd after saving
```



### Hide  _cmds from history

```bash
# add to .bashrc
# Don't add duplicate lines or lines beginning with a space to history
HISTCONTROL=ignoreboth

# add space before cmd
<_space_>apt update
```



### Включить отметку времени в истории bash 

```bash
export HISTTIMEFORMAT="%Y-%m-%d %T "
# or
HISTTIMEFORMAT="%d/%m/%y %T "

history
# И теперь он также сообщит вам, когда вы выполнили определенную команду в истории
```

### Поиск команд через сервис поиска прошлых команд

```bash
Ctrl + R и вводим часть команды которую ищем

ALT + R чтобы восстановить начальное состояние найденной команды
```

## List the most used commands

With the following command, we can see which have been the most frequently executed commands during the different shell sessions.

```
$ history |  awk '{a[$2]++}END{for(i in a){print a[i] " " i}}' |  sort -rn |  head
```
