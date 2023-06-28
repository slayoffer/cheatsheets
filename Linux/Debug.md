The `set -x` command is used to turn on debugging in a shell script and can also be used to test bash aliases. When `set -x` is used, the command and its arguments are printed to the standard error stream before the command is executed. This can be useful for testing aliases because it lets you see exactly what command is running and with what arguments.

Here’s an example of how you can use `set -x` to test a bash alias:

```
# turn on debugging
set -x

# create an alias called "update" that runs the command "apt-get update"
alias update='apt-get update'

# run the "update" alias
update

# turn off debugging
set +x
```

When you run this script, you should see the command `apt-get update` And its arguments are printed to the standard error stream before the command is executed. This allows you to see that the alias is working correctly and that the correct command and arguments are being used.

***Note\***: *It's important to remember that after you finish debugging, you need to turn off the debugging with set +x. Otherwise, any command you run will be printed to the standard error stream, generating a lot of noise and making it harder to use your terminal.*