The `xxd` utility is one of the multitude of ways to perform hexadecimal dumps on Linux. There are a ton of utilities that have similar functionality, but the `xxd` program is slightly different. The added advantage is that you can both dump and restore hex using this utility. There are also a lot of configurable flags and you can perform patches on binary files as well.

Let’s assume we wanted to take a hex dump of the following file named `foo`:

```
1234
5678
```

All we have to do is provide the input and `xxd` will encode the file to `stdout` automatically (a handy feature to have by default for shorter input files):

```
xxd foo

00000000: 3132 3334 0a35 3637 380a                 1234.5678.
```

You can also output directly to a dump file by passing an additional filename parameter:

```
xxd foo bar
```