### Create

```yml
resource "aws_instance" "webserver" {
    ami = "ami-@edab43b6fa892279"
    instance_type = "t2.micro"
    tags = {
        Name = "webserver"
        Description = "An Nginx WebServer on Ubuntu"
	}
	
	user_data = <<-EOF
    #!/bin/bash
    sudo apt update
    sudo apt install nginx -y
    systemctl enable nginx
    systemctl start nginx
	EOF
	# or
	user_data = file("./install-nginx.sh")
	
	# or use provisioner module for running code on EC2 bootup (NOT RECOMMNEDED)
	provisioner "remote-exec" {
        inline = [ "sudo apt update",
                   "sudo apt install nginx -y",
                   "sudo systemctl enable nginx",
                   "sudo systemctl start nginx",
                 ]
	}
	# but provisioner module needs connection settings to run
    connection {
        type = "ssh"
        host = self.public_ip
        user = "ubuntu"
        private_key = file("/root/.ssh/web")
    }
    
	key_name = aws_key_pair.web.id # check key creation below
	vpc_security_group_ids = [ aws_security_group.ssh-access.id ] # check group creation below
	
	provisioner "local-exec" { # to get external ip without using datasource
		on_failure = continue # will not stop terraform apply even if this script fails to run
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
# for ssh key
resource "aws_key_pair" "web" {
	key_name = "cerberus"
    public_key = file("/root/.ssh/web.pub") # or just add text content of key file in double brackets
}

# create security group
resource "aws_security_group" "ssh-access" {
    name = "ssh-access"
    description = "Allow SSH access from the Internet”
	ingress {
        from_port = 22
        to_port = 22
        protocol = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
	}
}

# get static public ip
resource "aws_eip" "eip" {
  vpc      = true
  instance = aws_instance.cerberus.id
  provisioner "local-exec" {
    command = "echo ${aws_eip.eip.public_dns} >> /root/cerberus_public_dns.txt"
  }
}

# get ip
output publicip {
	value = aws_instance.webserver.public_ip
}
```

