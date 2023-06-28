Servers generally behave, but sometimes things get installed on them that shouldn’t be there. It could be other users, configuration management software or just you flubbing something on the keyboard, but the wrong things do get installed. This means fighting with *where* exactly things are. That is sometimes easier said than done.

Thankfully, on Linux we have the `which` utility. This helps us determine “which” version of a program we are running. Clever name, right?

Let’s say you have multiple different versions of the same program or language installed. You aren’t exactly sure where it is being run from in the environment. Maybe it’s in `/usr/bin` or maybe it’s `/usr/local/bin`? Don’t waste time hunting it down manually, just figure it out with `which`:

```
which echo

>>> /usr/bin/echo
```

As you can see, we ask `which` version of `echo` we’re using and we get back the current path for that executable. Of course, it is unlikely we “install” a different version of `echo` but this can be really useful for things like programming languages and interpreters. Multiple versions are frequently in play on some servers.