### Implicit dependency (reference expression)

```bash
 resource "time_static" "time_update" { # will generate time id
}

 resource "local_file" "time" {
     filename = "/root/time.txt"
     content = "Time stamp of this file is ${time_static.time_update.id}" # will fetch time id from time resource
}

# or
resource "local_file" "file" {
    filename = var.filename
    file_permission =  var.permission
    content = random_string.string.id # implicitly depends on random_string
}

resource "random_string" "string" {
    length = var.length
    keepers = {
        length = var.length
    }  
}

terraform show
```

### Explicit dependency

```bash
 resource "local_file" "whale" {
     filename = "/root/whale"
     content = "whale"
     depends_on = [
         local_file.krill
     ]
}

 resource "local_file" "krill" {
     filename = "/root/krill"
     content = "krill"
}
```

