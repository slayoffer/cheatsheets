
### Create screen

```bash
screen -S my_screen
```

### Attach to screen

```bash
screen -r 61929.pts-0.localhost
# or
screen -raAd
```

### Example

```bash
I=0 && while true; do clear; echo $I; let I=${I}+1; sleep 10; done
```

### Show sessions list

```bash
screen -ls
```

### Detach from screen

```bash
Ctrl + a + d # [detached from 297970.pts-0.fhmfg0qh8c9nu2m43rbc]
```

### Join a session without detaching

```bash
screen -x 61929.pts-0.localhost
```



### Create screen with name

```bash
screen -S my_screen
```

### Keys

```bash
# screen позволяет удобно запускать несколько оболочек в одном окне. Вот краткий список горячих клавиш:
Ctrl+a c — создать ещё одну оболочку
Ctrl+a " — просмотреть список оболочек в текущем сеансе screen
Ctrl+a NUM — переключиться на NUM окно, где NUM - это номер окна
Ctrl+a A — переименовать окно
Ctrl+a S — разделить окно горизонтально
Ctrl+a | — разделить окно вертикально
Ctrl+a tab — переключиться в следующий терминал в пределах окна
Ctrl+a Ctrl+a — переключиться между предыдущим окном и текущим
Ctrl+a X — закрыть текущую оболочку
Ctrl+a Q — закрыть всё, кроме текущей оболочки
```

