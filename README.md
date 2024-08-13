
AWS Project based on Load Balncer:-
-------------------------------------
steps:
--: Create VPC:- and wait till complete the creation vpc mode, check the resource map all the subnets private and public.
    Here the Private subnets are to create EC2 machines,these are used to  deploy the atually applications,public subnets are used to create a ec2 machine make use this machine and will connect to private subnet machine, it will act as bastion server.
    will add a nat gateway: It will used to mask the private ip address and will expose  the public ip.
    NAT (Network Address Translation) gateway is a device or service that allows multiple devices within a private network to share a single public IP address.
--: Created new vpc: aws-prod-example.
    Create Auto scaling group:-  We can’t create the auto scaling group directly,
    Will create first lunch template, make use this template and create auto scaling group.
 
--: Application LoadBalance :- Before going to application load balancer, install the applications into the instances.
      Here these instances are private so we will not access directly .will use bastion host.(public subnet)
      Ssh bastion and copy the key-value into the bastion then only will access the private ip instances.
      login into the private machine and create basic python hml application.
        #1.create index.html page.
      #2.python -m http.server 8000(run from same private machine)
    -->aatch load balancer(http &https l7)create target group here .
    Note :--LB port will use and security port open with 8000.
    --to access the application in browse use LB ARN.
    --Elestic ip address is a static ip, which won’t be change the ip if instance go down and up also.


![image](https://github.com/naveenit2017/aws-cloud/assets/114174494/f411a5ee-90b4-4da8-ae27-f87a56251432)

IAM --Identity access Management
Root account will control the below things.
--IAM Permissions
--Billing
--Resources.

users:
#create IAM user user_naveen and through thatlogin.
 --> try to access a s3 bucket.
---> create policy and attach that policy to naveen_user now you are able to access the s3 bucket.
Groups:
#create a demo_group and add the users to that group.
--add the policies at group level,Hence all the users can inherit the properties from group.

AWS Organizations:
------------------
From the root account will create sub aws accounts,we can create OU(Organizational Units).eg:Dev,Qed,Prod
--If we don't want to impact with other teams will go with isolation environments .

AssumeRole:Assume role is a powerful feature in AWS Identity and Access Management (IAM) that allows users or applications to 
temporarily obtain the permissions associated with a different role. This is useful for various scenarios such as granting temporary access, 
delegating permissions to different AWS accounts, or performing cross-account access. 

user  + Role + Policy.
1.create test_user
2.create a role--aws account--add permissions --amazons3fullaccess--role-name(s3-full-access)
3.go to test_user--add permissions--inline-policy--json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Statement1",
			"Effect": "Allow",
			"Action": "sts:AssumeRole",
			"Resource": "arn:aws:iam::992382502754:role/s3-fully-access"  //ARN of s3bucket role.
		}
	]
}
use the console url and access the temporary access.

Task: Bastion Server or Jumphost server.

Bastion and Jump Server are both tools used for secure remote access to servers and networks. 
#Bastion acts as a secure gateway for accessing servers and requires users to authenticate before gaining access. 
#Jump Server, on the other hand, is a dedicated server that acts as a middleman between the user and the target server, providing an additional layer of security. 
 While both tools serve similar purposes, Jump Server offers more control and monitoring capabilities, making it a preferred choice for organizations with stricter security requirements.

Practical:
---------
1.create VPC
2.create IGW and attach to VPC
3.create public-rt & private-rt
4.create public and private subnets
5.route table assosiation --public subnet for public-rt and edid routes provide the internet access through igw.
6.route table assosiation --private subnet for private-rt don't provide the internet access to private subnet.
7.create the public ec2 in public subnet and private ec2 in private subnet.
8.and shh into public ec2 and from there ssh to private ec2.
9.But private ec2 don't have the internet access.
NAT Gateway(Network Adress transition):To provide the private ec2 machines internet access.
--------------------------------------
1.Create NAT Gateway
2.select subnet
3.connectivity type public.
4.attach to private rout table 
5.No check ping command your able get the internet access .

userdata:while creating the ec2 machines or after creating the ec2 machines if we want to perform some kind of acctions 
--------
will write the peace of code or shell scripts we can write or upload it perfom the action based inputs.
EC2 launch template:It is a template to define all the required parameters to create ec2 machine,make use of this template will create a machine.
--------------------
AWS LB:- Project
-------
steps :
1.create vpc
2.Create a IGW and attach to VPC
3.create a two subnets in diffrent availabilty zones.
4.create route table and associate subnets
5.edit routes and provide the internet access to the RT.
6.create 2 ec2 machines in different availabilty zones make use above create subnets.
7.while creating add userdata to test our ALB.
Target Group :
1.create targetgroup & selct target type,instances or ipaddresses
2.select VPC
3.protocol version HTTP
4.select instances --etc.
ALB:
1.create LB 
2.select VPC
3.select AZ
4.select security groups which are associated with instances
5.create 
------------------------
Autoscaling:
----------
1.create vpc
2.Create a IGW and attach to VPC
3.create a two subnets in diffrent availabilty zones.
4.create route table and associate subnets
5.edit routes and provide the internet access to the RT.
AutoScaling Group:
-----------------
6.launch template
7.provide all the details.



AWS Inspector:
-------------
Amazon Inspector is a security assessment service that helps improve the security and compliance of applications deployed on AWS. 
It automatically assesses applications for exposure, vulnerabilities, and deviations from best practices.
Inspector scans EC2 instances and container images in Amazon ECR for known vulnerabilities, unintended network exposure, and insecure configurations.

![image](https://github.com/user-attachments/assets/9917a4dc-fcf7-4cfa-be8f-b1a77319988a)



