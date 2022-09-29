# Create EC2 Instances

* From the AWS Console Home
  * EC2 > Launch Instance
* From the "Launch an instance" Wizard
  * Enter "TestEC2Instance" in the Name field
  * Ensure the Amazon Linux AMI is selected
  * Ensure t2.micro instance type is selected (or other free-tier option),
  * Key Pair > Create new key pair
    * Key Pair Name = Test
    * Key pair type = RSA
    * Ensure the type is .pem (if on Linux or MacOS, .ppk for Putty on Windows)
    * Click "Create Key Pair"
    * Ensure the Test.pem file is moved to your .ssh directory and you fix the permissions...

i.e.

```bash
chmod 600 .ssh/Test.pem 
```

  * Network Settings
   * Change the "Allow SSH traffic from" rule to use "My IP".
  * Configure Storage
   * Use defaults
  * Advanced Details
   * In the User data field add...

```bash
#!/bin/bash
yum install -y amazon-cloudwatch-agent
yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
```

  * Summary
   * Number of instances = 3
  * Click "Launch instance"
  * Click "View all instances" and wait for the status checks to stabilize.

# Create a IAM Role for Cloudwatch and assign the instances

* In the EC2 Console select an instance
* Actions > Security > Modify IAM role > Create new IAM role
* Click "Create Role"
* Select "AWS service"
* Use case > EC2
* Click "Next"
* Search for the CloudWatchAgentServerPolicy policy and ensure it is checked
* Search for the AmazonSSMManagedInstanceCore policy and ensure it is checked
* Search for the CloudWatchAgentAdminPolicy policy and ensure it is checked - (This one is only needed temporarily, remove it from the role later from the role)
* Scroll down and click Next
* Role Name = EC2CloudWatchSSM
* Scroll down and click "Create Role"
* Return to the previous page for the EC2 Instance (Modify IAM role page)
* Click the refresh icon next to the drop down
* Select the EC2CloudWatchSSM role
* Click "Update IAM Role"
* Assign the role to the remaining instances using the Actions > Security > Modify IAM Role menu option

# Create CloudWatch Agent config file with the Wizard

* Logon to an EC2 instance (Session Manager should work)
* sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
* Follow the Wizard - defaults should be ok.
* Configure at least one log file to monitor, i.e. /var/log/messages
* Save the configuration to the Systems Manager Parameter Store (default name would be AmazonCloudWatch-linux)
* Run the CloudWatch Agent `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c ssm:AmazonCloudWatch-linux`
* Run the above command on the remaining instances to configure the CW Agent

# Remove CloudWatchAgentAdminPolicy from the EC2CloudWatchSSM Role

* IAM > Roles
* Search for EC2CloudWatchSSM
* Click the role name
* Check the "CloudWatchAgentAdminPolicy" policy > Remove

# Clean Up

* EC2 > Terminate 3 Instances
* IAM > Search for "EC2CloudWatchSSM" > Check Box > Delete > Type the role name in the box to confirm > Delete
* CloudWatch > Logs > Log Groups > Select "messages" group > Actions > Delete log group(s)
* EC2 > Key Pairs > Check "Test" > Actions > Delete (Can keep this if you want to reuse it)