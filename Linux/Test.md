### Test if file exists

```bash
$ touch example
# -e for exists
$ test -e example && echo "foo" # exists

foo

$ test -e notafile && echo "foo" # not exists

$

# or
$ touch example

$ test -e example || echo "foo"

$ test -e notafile || echo "foo"

foo

$
```

### Another way to test

```bash
$ touch example

$ [ -e example && echo "foo"

foo

$ [ -e notafile && echo "foo"

$
```

### Testing for file types

```bash
-f: regular file (returns false for a directory)

-d: directory

-b: block (such as /dev/sda1)

-L or -h: symlink

-S: socket
```

### Testing for file attributes

```bash
-s: a file with the size greater than zero

-N: a file that's been modified since it was last read

# You can test by ownership:

-O: a file owned by the current primary user

-G: a file owned by the current primary group

# Or you can test by permissions (or file mode):

-r: a file with read permission granted

-w: a file with write permission granted

-x: a file with execute permission granted

-k: a file with the sticky bit set
```

### Combining tests

```bash
$ touch zombie apocalypse now

# -a for combine
$ test -e zombie -a -e apocalypse -a -e now && echo "no thanks"

no thanks

The -o option ("or") requires that one expression is true:

$ touch zombie apocalypse now

$ test -e zombie -o -e plant -o -e apocalypse && echo "no thanks"

no thanks
```

### Integer tests

```bash
-eq: equal to

-ne: not equal

-ge: greater than or equal to

-gt: greater than

-le: less than or equal to

-lt: less than

Here's a simple example:

$ nil=0

$ foo=1

$ test $foo -eq $nil || echo "Those are not equal."

Those are not equal.

$ test $foo -eq 1 && echo "Those are equal."
Of course, you can combine tests.

$ touch example

$ test $foo -ne $nil -a -e example -o -e notafile && echo "yes"
yes
```

