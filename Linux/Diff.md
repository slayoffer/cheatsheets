### Diff command

```bash
# check difference between files
diff file1.txt file2.txt
# of with verbose
diff -v file1.txt file2.txt

# first result line shows line numbers (15c15) - line 15 in first, line 15 in second file, c for changed, a for add, d for delete
```

### Patch mode

```bash
# with detailed info
diff -u file1.txt file2.txt

# Одной из особенностей diff является использование опции «-u» для создания патчей. Которые могут быть использованы для приведения содержимого файла к актуальному состоянию. Это очень удобно, ведь все инструкции для актуализации содержатся в одном файле-патче, который может быть обработан автоматически с помощью специализированной утилиты, например patch. Вывод для примера с использованием опции «-u» будет следующим:
diff -u file1 file2
--- file1 2019-04-08 20:11:51.051674991 +0400
+++ file2 2019-04-08 20:11:39.059202352 +0400
@@ -1,3 +1,6 @@
line
-line2
+line11
+line22
line3
+line4
+line5
```

### Diff with color

```bash
which colordiff
colordiff file1.txt file2.txt
# better to create alias for colordiff instead of diff
```

### Explanation

```bash
# Теперь, для более наглядной демонстрации работы утилиты diff можно несколько усложнить условия задачи из предыдущей главы, изменив файл №2 следующим образом:
line1
line11
line22
line3
line4
line5
# Тогда, выполнив команду
diff file1 file2
# будет получен следующий вывод:
2c2,3
< line2 --- > line11
> line22
3a5,6
> line4
> line5
# Здесь выражение «2c2,3» говорит о том, что второй строке (line2) в файле file1 соответствуют изменения в file2 в виде строк «line11» и «line22». Запись «2,3» означает диапазон строк в file2, содержащих изменения относительно строки №2 в file1. Запись «3c5,6» говорит о том, что после строки №3 оригинального файла в его изменённой версии (file2) были добавлены строки № 5 и №6, т. е. «line4» и «line5».
```

