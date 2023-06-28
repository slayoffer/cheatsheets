### Instance deploy example

```bash
# create SSH key
aws ec2 create-key-pair --key-name myCliKeypair --query 'KeyMaterial' --output text > myCliKeypair

# create security group
aws ec2 create-security-group --group-name my-sg --description "My security group"

# add inbound rule for SSH and my ip
aws ec2 authorize-security-group-ingress --group-id sg-0cc8b35f0484ae838 --protocol tcp --port 22 --cidr 95.154.71.237/32

# add rule for all ips
aws ec2 authorize-security-group-ingress --group-id sg-0cc8b35f0484ae838 --protocol tcp --port 22-8000 --cidr 0.0.0.0/0

# check all sec groups
aws ec2 describe-security-groups

# create instance
aws ec2 run-instances --image-id ami-08c40ec9ead489470 --count 1 --instance-type t2.micro --key-name myCliKeypair --security-groups my-sg

# create tag for instance
aws ec2 create-tags --resources i-045c6fb7ac2104bc0 --tags Key=Owner,Value=DevOps
```