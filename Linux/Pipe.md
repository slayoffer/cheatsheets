### No-name pipe (basic)

```bash
ls | grep file | grep -v ssh 

#
cut -d: -f1 < /etc/passwd | sort | grep u > /tmp/text
# Копируем содержимое файла /etc/passwd в оперативную память и передаём программе cut. Она отрезает часть строки, оставляя только логины, а результат передаётся команде sort. После сортировки вывод подхватывает команда grep на фильтрацию по наличию символа u. Итог записывается в файл /tmp/text
```

### Named pipe

```bash
mknod test_pipe p
# or
mkfifo test_pipe

cat file1 > test_pipe

Ctrl+c

ls -l test_pipe
```

## Execute command without asking for confirmation

If we want to execute a Linux command without being asked for confirmation at each step, we can use “yes” as follows:

```
$ yes | sudo apt install XXX
```
