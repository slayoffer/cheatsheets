### Simple

```bash
output "os-version" {
  value = data.local_file.os.content
}
data "local_file" "os" {
  filename = "/etc/os-release"
}
```

### AWS

```bash
data "aws_ebs_volume" "gp2_volume" {
  most_recent = true

  filter {
    name   = "volume-type"
    values = ["gp2"]
  }
}

#

data "aws_s3_bucket" "selected" {
  bucket_name = "bucket.test.com"
}
```

