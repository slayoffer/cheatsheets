### Provisioners are not recommended for use!

```bash
# better use user_data (AWS), custom_data (Azure), meta_data (GCP), etc

# or even better use AMIs with all the needed apts installed and dont use these modules at all (build custom AMIs via Packer, etc)

resource "aws_instance" "webserver" {
    ami = "ami-@edab43b6fa892279"
    instance_type = "t2.micro"
    tags = {
        Name = "webserver"
        Description = "An Nginx WebServer on Ubuntu"
	}
	
	provisioner "remote-exec" {
        inline = [ "sudo apt update",
                   "sudo apt install nginx -y",
                   "sudo systemctl enable nginx",
                   "sudo systemctl start nginx",
                 ]
	}
	# provisioner module needs connection settings to run
    connection {
        type = "ssh"
        host = self.public_ip
        user = "ubuntu"
        private_key = file("/root/.ssh/web")
    }
	
	provisioner "local-exec" { # to get external ip without using datasource
		on_failure = continue # will not stop terraform apply even if this script fails to run
		# or
		on_failure = fail # default setting
		command = "echo ${aws_instance.webserver.public_ip} >> /tmp/ips.txt"
	}
	
    provisioner "local-exec" { # to inform about EC2 creation
    command = "echo Instance ${aws_instance.webserver.public_ip} Created! > /tmp/instance_state.txt"
	}
	
	provisioner "local-exec" { # to inform about EC2 deletion
		when = destroy
		command = "echo Instance ${aws_instance.webserver.public_ip} Destroyed! > /tmp/instance_state.txt"
	}
}
```

