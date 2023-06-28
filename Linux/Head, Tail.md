## Head and tail to show part of text files

If you only want to see certain parts of the text file in cat-styled display, use the head and tail commands.

By default, the head command displays the first 10 lines of a file.

```
head filename
```

But you can modify it to show the first n lines as well.

```
head -n filename
```

The tail command displays the last 10 lines by default.

```
tail filename
```

But you can modify it to show n lines from the bottom.

```
tail -n filename
```

### Practice examples

Let's see some examples. Generate an easy-to-follow file using this script:

```
#create or clear the content of the file
echo -n > sample

#put content to the file
for i in {1..70}
do
  echo "This is the line $i" >> sample
done
```

Create a new file named script.sh and copy-paste the above script content into it. Now run the script like this to generate your sample file:

```
bash script.sh
```

Now, you have got a file named `sample` that contains lines like "This is the line number N" for every 70 lines.

🖥️

Display the first 10 and the last 10 lines of this sample file.

Let's take it to the next level. You can combine them both to show specific lines of a file. For example, to show lines from 35 to 40, use it like this:

```
head -n 40 filename | tail -n +35
```

Here:

- `head -n 40 filename` will display the first 40 lines of the file.
- `tail -n +35` will display the lines from the 35th line to the end of the output from the `head` command. Yeah! Mind the + sign that changes the normal behavior of the tail command.

You can also combine them to show only a particular line. Let's say you want to display the 55th line; combine head and tail like this.

```
head -n 55 filename | tail -n 1
```

Here:

- `head -n 55 filename` will display the first 55 lines of the file.
- `tail -n 1` will display the last line of the output from the `head` command, which will be the 55th line of the file.