### Set provider version

```bash
terraform {
	required_providers {
    	local = {
            source = "“hashicorp/local"
            version = "2.0.0"
            # or
            version = "!= 2.0.0"
            # or
            version = "> 1.4.0"
            # or
             version = ">= 1.4.0"
            # or
            version = "> 1.4.0, < 2.0.0, != 1.4.0"
            # or
            version = "~> 1.2" # will either download 1.2 or next version up to 1.9
            # or
            version = "~> 1.2.0" # will either download 1.2.0 or next version up to 1.2.9
		}
	}
}

resource “local_file" "pet" {
    sari = "/root/pet.txt"
    content = "We love pets!"
}
```

