### Examples

```bash
sgpt "mass of sun"

sgpt --chat joke "can you tell me a joke?"

sgpt --code "Solve classic fizz buzz problem using Python"

# without execution (just show how to do it)
sgpt --shell "Make all files in the current directory read-only"

# with execution (do it for me)
sgpt --shell --execute "make all files in current directory read-only"
```

| `**--temperature**`    | Changes the randomness of the output                         |
| ---------------------- | ------------------------------------------------------------ |
| `**--top-probablity**` | Limits to only the highest probable tokens or words          |
| `**--chat**`           | Used to have a conversation with a unique name               |
| `**--shell**`          | Used to get shell commands as output                         |
| `**--execute**`        | Executes the commands received as output from `--shell` option |
| `**--code**`           | Used to get code as output                                   |