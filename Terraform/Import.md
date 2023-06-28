### Import flow

```bash
# at first prepare resource block
resource "aws_instance" "webserver-2" {
# (no resource arguments)
}

# then import the resource
terraform import aws_instance.webserver-2 i-026e13be10d5326f7 # ID of instance

# add webserver data from updated state (check state file for details) - IT WONT BE DONE AUTOMATICALLY !!!
resource "aws_instance" "webserver-2" {
    ami = "ami-@edab43b6fa892279"
    instance_type = "t2.micro"
    key_name = "ws"
    vpc_security_group_ids = ["sg-8064fdee"]
}
```

