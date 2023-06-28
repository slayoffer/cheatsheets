### Запуск команды в фоновом режиме

``` bash
# Поставить амперсант (&) в конец команды
tar xjf archive.tar.bz2 &

# Если хотим, чтобы команда продолжала работать даже после закрытия терминала, то добавляем в начало команды аргумент nohup
nohup tar xjf archive.tar.bz2 &
```

The `&` at the end of a command in Unix-like operating systems (like Linux) is used to run the command in the background.

If you use `nohup` but do not add the `&`, the command will not return control to the shell immediately. Instead, it will block the shell and wait for the command to finish. Although it would still be protected from the hangup signal (meaning it won't stop if the terminal window is closed), it won't return the command prompt to you until it's done.

So, typically when you use `nohup`, you also use `&` to allow the command to run in the background, freeing up the terminal for other use. This is particularly useful for long running commands, which is a common use case for `nohup`.

But, to answer your question directly, it's not strictly necessary to use `&` with `nohup`, but in most cases you would want to.