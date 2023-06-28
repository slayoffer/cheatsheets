### Configure

```bash
touch main.tf

resource "local_file" "state" {
  filename = "/root/${var.remote-state}"
  content  = "This configuration uses ${var.remote-state} state"
}

touch terraform.tf
#input
terraform { # this module will send local state to remote s3 bucket
  backend "s3" {
    key = "terraform.tfstate"
    region = "us-east-1"
    bucket = "remote-state"
	dynamodb_table = "state-locking" # needed for state locking
	
    endpoint = "http://172.16.238.105:9000" # not needed for AWS
    force_path_style = true # not needed for AWS
    skip_credentials_validation = true # not needed for AWS
    skip_metadata_api_check = true # not needed for AWS
    skip_region_validation = true # not needed for AWS
  }
}

terraform init # even if already did, need to do again
```

### Create DB for state locking

```bash
resource "aws_dynamodb_table" "state-locking" {
    name = "state-locking"
    billing_mode = "PAY_PER_REQUEST"
    hash_key = "LockID"
	attribute {
		name = "LockID"
		type = "S"
	}
}
```

