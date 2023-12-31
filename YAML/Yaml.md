### YAML

Ansible использует декларативное описание выполняемых задач в YAML-файлах. 

**YAML** — это язык представления данных, которые отображаются в виде скалярных переменных (содержат одно значение), списков, словарей.

Каждый элемент списка выделяется с помощью тире:

```bash
my_list:
  - первый элемент списка
  - второй элемент списка
  - еще один элемент 
```

Или в однострочной нотации в квадратных скобках:

```bash
my_list: ["первый элемент списка", "второй элемент списка", "еще один элемент"] 
```

Словарь в YAML — это набор ключей и их значений:

```bash
# Просто переменная
answer: 42

# Переменная, в которой лежит словарь
person:
  name: Marvin
  origin: Paranoid Android
  motd: Don't Panic 
```

Или в однострочной нотации в фигурных скобках:

```bash
# Переменная, в которой лежит словарь
person: {name: Marvin, origin: "Paranoid Android", motd: "Don't Panic"} 
```

И, наконец, можно организовать список таких словарей:

```bash
droids:
  - name: C3PO
    color: gold
    spec:
      humor_level: "1%"
      memory_size: "640KB"
  - name: R2D2
    color: blue
    spec:
      humor_level: "95%"
      memory_size: "128PB" 
```

Современные IDE, такие, как Visual Studio Code, подсвечивают и проверяют синтаксис YAML. В CI можно выполнять проверку синтаксиса с помощью линтера `yamllint`, который также проверит повторения ключей, отступы, длину строк.