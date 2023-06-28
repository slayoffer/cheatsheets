## Execute in one step a pull request in all git directories

A quick way to update all our git repositories is to run the following command:

```
$ for i in */.git; do cd $(dirname $i); git pull; cd ..; done
```