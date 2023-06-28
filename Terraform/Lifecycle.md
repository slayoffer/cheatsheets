### Create before destroy, Prevent destroy

```bash
resource "local_file" "pet" {
    filename = "/root/pets.txt"
    content = "We love pets!"
    file_permission = "0700"

    lifecycle {
    create_before_destroy = true
	}
	
resource "local_file" "pet" {
    filename = "/root/pets.txt"
    content = "We love pets!"
    file_permission = "0700"

    lifecycle {
    prevent_destroy = true # great for DBs
	}
```

### Persist configuration changes (e.g. config drift)

```bash
    lifecycle {
    ignore_changes = [
    	tags, ami, content
    	# or
    	all # for all changes
    	]
	}
```

