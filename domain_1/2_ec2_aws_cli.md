# Set the AWS Profile if appropriate

```bash
export AWS_PROFILE=myprofile
```

# Create a key pair

```bash
aws ec2 create-key-pair --key-name EC2Test --query 'KeyMaterial' --output text > EC2Test.pem
mv EC2Test.pem .ssh/
chmod 600 .ssh/EC2Test.pem
```

# Create Security Group

```bash
aws ec2 create-security-group --group-name ssh --description "Allow incoming SSH"
# Record the returned group id
export SG=XXXXXXX;
MYIP=$(curl https://checkip.amazonaws.com)
aws ec2 authorize-security-group-ingress --group-id "$SG" --protocol tcp --port 22 --cidr "$MYIP/32"
```

# Create a IAM Role for Cloudwatch and assign it to the instances

```bash
cat << EOF > EC2CloudWatchSSM.json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "ec2.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
EOF
aws iam create-role --role-name "EC2CloudWatchSSM" --assume-role-policy-document file://EC2CloudWatchSSM.json
aws iam attach-role-policy --role-name "EC2CloudWatchSSM" --policy-arn arn:aws:iam::aws:policy/CloudWatchAgentAdminPolicy  # temp, can be removed after setup
aws iam attach-role-policy --role-name "EC2CloudWatchSSM" --policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy
aws iam attach-role-policy --role-name "EC2CloudWatchSSM" --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
aws iam create-instance-profile --instance-profile-name "EC2CloudWatchSSM"
aws iam add-role-to-instance-profile --instance-profile-name "EC2CloudWatchSSM" --role-name "EC2CloudWatchSSM"
```

# Create EC2 Instances

```bash
cat << EOF > user-data.txt
#!/bin/bash
yum install -y amazon-cloudwatch-agent
yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
amazon-linux-extras install collectd
EOF

export AMI="ami-XXXXXXXXXXXXXXX";
aws ec2 run-instances --image-id "$AMI" --count 3 --instance-type t2.micro --key-name EC2Test --security-group-ids "$SG" --iam-instance-profile Name="EC2CloudWatchSSM"  --user-data-file file://user-data.txt  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=TestEC2Instance}]'  -- --subnet-id (required??)
```

# Create CloudWatch Agent config file with the Wizard

* See 1_ec2_web_console.md document.

# Clean Up

```bash
aws ec2 delete-key-pair --key-name EC2Test
aws ec2 delete-security-group --group-id "$SG"
aws iam delete-instance-profile --instance-profile-name "EC2CloudWatchSSM"
aws iam delete-role --role-name "EC2CloudWatchSSM"
aws ec2 delete-instance --instance-name "TestEC2Instance"
```

and if required...

```bash
rm .ssh/EC2Test.pem
```