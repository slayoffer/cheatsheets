First up is a really neat little utility that will help you wrap input lines to specific lengths. The length can be defined in bytes or spaces. Using the `fold` utility you can quickly tame files with varying run-on lengths.

For example, let’s assume we have an input line that is six characters long. We want to limit each line to only five characters and wrap the remainder. Using `fold` we can do this with the following command:

```
echo "123456" | fold -w 5
```

The corresponding output should be:

```
12345
6
```

Now we can quickly force some text to conform to our length restrictions. This is really handy if you want to break down long streams of text or enforce line length limitations on code or other configuration files.