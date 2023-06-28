### Find and move files

```bash
# move all .txt files to /new_dir
find . -type f -name "*.txt" | xargs -t -I{} mv {} ../new_dir
```

