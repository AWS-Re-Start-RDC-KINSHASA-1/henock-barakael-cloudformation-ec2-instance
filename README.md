To launch an EC2 instance with user data using CloudFormation, follow these step-by-step instructions:

Step 1: Create a CloudFormation template file

Create a new file named `ec2-instance.yaml` and copy the following contents into it:

```yaml
---
Resources:
  SSHSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Enable SSH and HTTP access
      SecurityGroupIngress:
        - CidrIp: 0.0.0.0/0
          FromPort: 22
          ToPort: 22
          IpProtocol: tcp
        - CidrIp: 0.0.0.0/0
          FromPort: 80
          ToPort: 80
          IpProtocol: tcp

  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      AvailabilityZone: us-east-1a
      ImageId: ami-a4c7edb2
      InstanceType: t2.micro
      KeyName: key
      SecurityGroupIds:
        - !Ref SSHSecurityGroup
      Tags:
        - Key: Name
          Value: CloudFormationInstance
      UserData:
        Fn::Base64: !Sub |
          #!/bin/bash
          yum update -y
          yum install -y httpd
          sudo service httpd start
          sudo service httpd enable
          echo "<html><body><h1>Welcome to my website!</h1><p>This is a custom message.</p></body></html>" > /var/www/html/index.html
          chmod 644 /var/www/html/index.html
```

In this template, we define two resources: a security group (`SSHSecurityGroup`) and an EC2 instance (`MyInstance`). The security group allows incoming SSH (port 22) and HTTP (port 80) traffic, while the instance is launched in the specified availability zone (`us-east-1a`), uses a specific Amazon Machine Image (AMI), and has a t2.micro instance type. The user data section contains a bash script that updates the instance, installs and starts the Apache HTTP server, creates a custom webpage, and sets appropriate permissions.

Step 2: Launch the EC2 instance using CloudFormation

1. Log in to the AWS Management Console and navigate to the CloudFormation service.
2. Click on "Create stack" or "Create stack with new resources" to start the stack creation process.
3. Choose "Upload a template file" and select the `ec2-instance.yaml` file you created.
4. Click "Next" to configure the stack details.
5. Provide a stack name and any other required parameters.
6. Click "Next" to configure stack options (if applicable).
7. Review the stack details and click "Create stack" to launch the EC2 instance.

CloudFormation will create the necessary resources based on the template. Once the stack creation is complete, you should have an EC2 instance running with the specified user data.

Step 3: Access the web server

1. Locate the EC2 instance created by CloudFormation.
2. Make a note of the Public IP or Public DNS of the instance.
3. Open a web browser and enter the Public IP or Public DNS in the address bar.
4. You should see the custom webpage with the "Welcome to my website!" message.

Congratulations! You have successfully launched an EC2 instance with user data using CloudFormation and accessed the web server running on that instance.
