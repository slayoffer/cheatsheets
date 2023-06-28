There are other ways to achieve the same thing using utilities like `awk` but the `column` utility is geared towards this specific use so that makes it incredibly simple to use, and remember the syntax for.

If we wanted to build a simple table based on a few lines of input, we could execute the following command:

```
echo -e "one two three\n1 2 3\n111111 222222 333322" | column -t
```

The output of this command should look like this:

```
one     two     three
1       2       3
111111  222222  333322
```

As you can see, the output is automatically formatted into neatly aligned columns. This forms a little table in the output and will automatically resize based on the length of each input line.