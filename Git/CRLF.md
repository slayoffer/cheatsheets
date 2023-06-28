### Turn off in Git

```bash
git config --global core.autocrlf false
```

### Turn off in Prettier

```json
# Try setting the "endOfLine":"auto" in your .prettierrc (or .prettierrc.json) file (inside the object)
# Or set
"prettier/prettier": [
  "error",
  {
    "endOfLine": "auto"
  }
]
```

### Turn off in Bash

```bash
sed -i 's/\r//' feature.sh
```