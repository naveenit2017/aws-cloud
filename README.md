
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

WAF(Web Application wirewall):
------------------------------
Steps:
-----
1.Create a VPC
2.create a internetgatway and attach to VPC.
3.create a public subnet
4.create route table and associate the public subnet with RT and route the internet.
5.create a ec2 machine using userdata
#!/bin/bash
yes | sudo apt update
yes | sudo apt install apache2
echo "<h1>Server Details</h1><p><strong>Hostname:</strong> $(hostname)</p><p><strong>IP Address:</strong> $(hostname -I | cut -d" " -f1)</p>" > /var/www/html/index.html
sudo systemctl restart apache2
6.take the ec2 instance public ip and try to acees the from the browser.
LoadBalancer:
--------------
7.create LoadBalncer Target Group
8.Create LB using that TG
9.use the LB DNS and try to access the application from browser.
WAF(Web Application Firewall):
------------------------------
10.web ACL Details.
11.select Region
12.go to left side tabs ipsets and create ipset rule.(use your computer ip eg.10.0.0.0/32)
13.use that and create WebAcl rule with default values
14.Take the LB DNS and test the application is able to access or not
![image](https://github.com/user-attachments/assets/9917a4dc-fcf7-4cfa-be8f-b1a77319988a)

VPC Peering:
-----------
1.Create a VPC-A
2.create a internetgatway and attach to VPC.
3.create a public subnet
4.create route table and associate the public subnet with RT and route the internet.
5.create a ec2 machine using userdata
#!/bin/bash
yes | sudo apt update
yes | sudo apt install apache2
echo "<h1>Server Details</h1><p><strong>Hostname:</strong> $(hostname)</p><p><strong>IP Address:</strong> $(hostname -I | cut -d" " -f1)</p>" > /var/www/html/index.html
sudo systemctl restart apache2
6.take the ec2 instance public ip and try to acees the from the browser.

1.Create a VPC-B
2.create a internetgatway and attach to VPC.
3.create a public subnet
4.create route table and associate the public subnet with RT and route the internet.
5.create a ec2 machine using userdata
#!/bin/bash
yes | sudo apt update
yes | sudo apt install apache2
echo "<h1>Server Details</h1><p><strong>Hostname:</strong> $(hostname)</p><p><strong>IP Address:</strong> $(hostname -I | cut -d" " -f1)</p>" > /var/www/html/index.html
sudo systemctl restart apache2
6.take the ec2 instance public ip and try to acees the from the browser.
Create VPC Peering connection :
-------------------------------
1.give peering coonection name
2.select source VPC
3.Select destination VPC
4.Establish the connection
5.Acceptence the peering connection -->go peering connection --actions--accept
6.change the route table routes vice versa.
7.test the coneection
8.login ec2 aand take the another machine private ip and try with curl <private-ip>

Transit Gateway:
----------------
To communicate with multiple VPCs will go with Transit Gateway,morethan two VPC peering is not good solution, we can implement.

![image](https://github.com/user-attachments/assets/242764a9-514d-46a3-9499-da8eb645d26f)
Implementaition:
---------------
create 3 VPCs A,B,& C follow below steps.(All the vpc should follow)
1.Create a VPC-A
2.create a internetgatway and attach to VPC.
3.create a public subnet
4.create route table and associate the public subnet with RT and route the internet.
5.create a ec2 machine using userdata
#!/bin/bash
yes | sudo apt update
yes | sudo apt install apache2
echo "<h1>Server Details</h1><p><strong>Hostname:</strong> $(hostname)</p><p><strong>IP Address:</strong> $(hostname -I | cut -d" " -f1)</p>" > /var/www/html/index.html
sudo systemctl restart apache2
6.take the ec2 instance public ip and try to acees the from the browser.
create transit gateway:
-----------------------
7.create a transitgatway my-vpc-a-b-c-transitgateway.
8.create tranistgateway attachement with all the vpc make use of transitgateway.
9.next go to the vpc routetables and add the sources of the vpcs cid range and should be the target is transitgateway  do it for all vpcs with all the vpcs vice versa.
10.login ec2 aand take the another machine private ip and try with curl <private-ip> you are able to access,without routes not able to connect.

NAT(Network Address Transilation) Gatway:
-----------------------------------------
![image](https://github.com/user-attachments/assets/08bb2964-8fd7-4e30-a357-d17f57a12b18)

Route53:
--------
Amazon Route 53 is a scalable and highly available Domain Name System (DNS) web service in AWS. It is designed to route end-user requests to applications, services,
and resources within AWS and beyond.

![image](https://github.com/user-attachments/assets/c3bbd6d0-a3e7-4ed1-aabb-6b16e94028f3)
Ater creating a hosted zone in AWS it will create 2 records by default.
1.NS Record
2.SOA
3.A record we have to create make use of our based requirement will use routing policy
eg.simple routing,weighted routing ..etc.
![image](https://github.com/user-attachments/assets/19558643-21cc-46ee-ab32-28e685e147d7)

ACM(AWS Certificate Manager):
----------------------------
AWS Certificate Manager (ACM) is a service provided by Amazon Web Services (AWS) that helps you provision, manage, and deploy SSL/TLS certificates for your AWS-based applications. SSL/TLS certificates are used to secure network communications and establish the identity of websites over the internet, ensuring data privacy and integrity.


![image](https://github.com/user-attachments/assets/275e6156-6a1d-4fd5-a7a8-381b68011098)


Creation of certificate:
Go to the ACM --Request certificate--Request a public Request(SSL/TLS) --provide a domain name--and select required fields as per your requirement and create.
Once the create of certficate select the certficate-->create records in Route53
Please go and observe the Route53 hosted zones records,there will be a record imported with type CNAME and if obsevre the ACM the criticate status will show issued.
Next: we need to change the load balncer listner by default wich is associated with http & 80,need open with https &443
As well as need to open https port in Security group also.
1.If type explictly it will go to the https otherwise it will go http.
2.Hence we can redirect the http to https--go to LB listners and select the ridirect and select with http and port should be 443.

VPC Endpoint:-
---------------
An Amazon VPC (Virtual Private Cloud) Endpoint is a feature in AWS that enables private connections between your VPC and supported AWS services and VPC endpoint services powered by AWS PrivateLink. 
This connection is made without the need to traverse the public internet, providing enhanced security and performance.
steps:-
------
1.search in aws console--VPC ENDPOINT
2.create endpoint(myendpoint)
3.service category(select aws services)
4.select services(eg:s3)
5.select type as gateway
6.select the vpc and private route table.
7.create vpcendpoint.
Now we are able to provide the internet access to the private subnets without having the public internet access.

![image](https://github.com/user-attachments/assets/565bd142-3584-4de6-99f0-3c73f9ffe10a)

Network LoadBalancer vs Application LoadBalancer:
-------------------------------------------------

Practical:
1.Create a VPC-Demo
2.create a internetgatway and attach to VPC.
3.create 2 public subnet and private subnets
4.create route table and associate the public subnet with RT and route the internet.
5.create 2 ec2 machine using userdata in 2 diffrent availability zones.
#!/bin/bash
yes | sudo apt update
yes | sudo apt install apache2
echo "<h1>Server Details</h1><p><strong>Hostname:</strong> $(hostname)</p><p><strong>IP Address:</strong> $(hostname -I | cut -d" " -f1)</p>" > /var/www/html/index.html
sudo systemctl restart apache2
6.take the ec2 instance public ip and try to acees the from the browser.

![image](https://github.com/user-attachments/assets/f63aa2b6-8990-40ac-b476-bc22afc2420e)

NLB--won't support the pathbased routing it will accept the only one targetgroup listners,won't possible multiple groups.(IT will work on OSI model Application Layer7 and support http,https)
ALB --Will support path based routing we can add the based on our requirement multiple targetgroups based on our requirement.(IT will work on OSI model Transport Layer 4 and support TCP,UDP)

![image](https://github.com/user-attachments/assets/55c37cd2-4109-437e-b5c6-55407bfeb3b8)

NLB,APLB and GWLB:
------------------
Network Load Balancer (NLB): Operates at Layer 4 (Transport Layer) of the OSI model. It balances traffic based on IP protocol data, such as TCP/UDP connections.
---------------------------
Application Load Balancer (ALB):
--------------------------------
Operates at Layer 7 (Application Layer) of the OSI model.It balances traffic based on application-level data, such as HTTP/HTTPS requests.
Gateway Load Balancer (GWLB):
-----------------------------
Operates at Layer 3 (Network Layer) of the OSI model. It is designed to route and load balance network traffic across virtual appliances, such as firewalls, intrusion detection systems, or other network security appliances.

AWS PrivateLink:
---------------
Suppose you have a VPC in which you run a critical application that needs to store data in Amazon S3. Instead of sending data to S3 over the public internet, you can set up an interface VPC endpoint using PrivateLink. This way, your application's traffic to S3 remains within AWS, increasing security and reducing latency.

Practical:
![image](https://github.com/user-attachments/assets/f28b9142-c6f4-45bd-96fb-09df6d71947f)

AWS Lambda:
-----------
![image](https://github.com/user-attachments/assets/e151bf5a-413a-49cb-be48-d4777d761e94)

What is lambda:
--------------
AWS Lambda is a serverless computing service provided by Amazon Web Services (AWS). It allows you to run code in response to events without provisioning or managing servers.
AWS Lambda automatically scales your application by running your code in response to each trigger, and you only pay for the compute time consumed.

Lamda function creation:
------------------------
import json
import boto3


def lambda_handler(event, context):
    # TODO implement
    print("Hello this is from lamda naveen ")
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda! sssssss')
    }
How to create a zip file to upload:
----------------------------------
1.write a code in visual studio 
2.run the python -m venv .(it will create a virtual environment with required dependencies)
3.go to the project locatiom and zip
4.create a lamda function in AWS
5.upload the zip file into aws
6.the lambda_handler function must be use like(test_lambda(it is a filename).lambda_handler)
7.test the code .
How to upload the zip code in s3:
-----------------------------------
1.select the same regions
2.create a s3 bucket and upload the zip
3.create the lambda function
4.pass the zip file ARN(from get it S3)
5.test it.

Lambda Layers:
-------------
Custom library,will put all are required dependecies togeather in one package or library which can be useful to multiple AWS Lamda functions.
![image](https://github.com/user-attachments/assets/544a0449-1a41-4f6e-8ce2-277c22973907)

**VPC Flowlogs:**
------------------
![image](https://github.com/user-attachments/assets/bc143333-c081-45e4-99c5-213695e93069)

VPC Flow Logs is an AWS feature that enables you to capture and monitor the network traffic data (IP traffic) flowing to and from your Virtual Private Cloud (VPC). Flow logs help you analyze traffic patterns, troubleshoot network issues, and ensure the security of your VPC resources.

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "vpc-flow-logs.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
use the above aasume role and attach the CloudWatchFullAccess policy.
#create IAM Roles --ec2 IAM role for (Demo-SSM-Role) using AmazonSSMManagedInstanceCore,create IAM Role with custom trusted policy
#create vpc,subnet,ec2 while creating ec2 in advanced use Demo-SSM-Role where profile section.
#create in Cloudwatch ,there create loggroup ,go vpc--create vpc-flow-logs by using the custom role.
https://github.com/acantril/learn-cantrill-io-labs/tree/master/00-aws-simple-demos/aws-vpc-flow-logs
------------------------------------

RDS(Raltional Database Service):
--------------------------------
Amazon RDS (Relational Database Service) is a managed relational database service provided by AWS. It simplifies the setup, operation, and scaling of a relational database in the cloud. RDS supports multiple database engines.
eg:Aura,mysql,oracle...etc.

Key Features of Amazon RDS:
---------------------------
Managed Service: AWS handles database administration tasks such as backups, patch management, and replication.
----------------
Scalability: Easily scale the database’s compute and storage resources.
------------
High Availability: RDS supports Multi-AZ deployments for high availability and read replicas for read scaling.
-----------------
Automated Backups: Automatic backups and snapshots can be configured, providing data protection.
-----------------
Security: RDS integrates with AWS Identity and Access Management (IAM) and can be isolated in a VPC for enhanced security.
---------
Performance: AWS provides optimized configurations and instance types tailored for database workloads.
-----------
Create RDS Database steps:
--------------------------
1.create a database 
2.select a method standard/easy
3.select Engine Options(Mysql,Oracle..etc)
4.select Engine version(Database version)
5.Templates(Dev,Prod,free tier)
6.username,password..vpc,replication.....etc
7.create the database.

Connect to RDS DB(mysql):
-------------------------
1.That machine should have mysql client
2.sudo mysql -h my-rd-database-1.cpss0s68exsu.us-east-2.rds.amazonaws.com -P 3306 -u root -p
3.create database(create database mydb)
4.make use the database(use mydb)
5.create table.

Migration on-primise or ec2 to RDS:
-----------------------------------
1.take the backup from ec2(dump of ec2)
2.mysql -u root -p  < testdump.sql
3.restore the backup(migrate)
4.mysql -h my-rd-database-1.cpss0s68exsu.us-east-2.rds.amazonaws.com -P 3306 -u root -p < testdump.sql
5.please login the RDS Database and check the respective database tables.

Migration DMS:(Data Migration Service)
--------------------------------------
Analasysis:(Before going to proceed Migration)
---------------------------------------------
#.Go to Environment and observe the existing infra.
#.Check around the environment
#.Talk to client
#.Ask Questions and know specifications
#.Alo ask for the access.
==Migration steps===============
1.create Replication instance
2.Endpoints create source and destination endpoints while creating please check the proper connection able to communicate.
3.Create database migration task


On-primse to AWS Cloud Migration:
--------------------------------
1.Here create EC2 machine in another region it will be act as a On-primse.
2.create Ec2 Machine in any one of the region(source machine).
3.host a webb application on that machine
4.create MGNuser and attach the below policy to user.
#AWSApplicationMigrationAgentPolicy
5.create accesskey & secretkey store it in note pad.
6.create a replication launch template in(AWS Migration Service) target location,i'e another region with default values.
7.Install AWS Agent on source machine by using below command
 #wget -O ./aws-replication-installer-init https://aws-application-migration-service-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/aws-replication-installer-init.py
 #sudo python3 aws-replication-installer-init.py
8.Go and observe the AWS Application Migration service console,it will lauch a soruce machine .
9.And follow the next steps for Cutover.

Amazon WorkDocs:
---------------
Amazon WorkDocs is a fully managed, secure content creation, storage, and collaboration service provided by AWS. It allows users to create, edit, share, and collaborate on documents, spreadsheets, presentations, and other content in real-time, similar to services like Google Drive or Microsoft OneDrive.

Amazon Workdrive:
-----------------
Amazon WorkDocs Drive is a feature of Amazon WorkDocs that allows users to access their Amazon WorkDocs files directly from their desktop as if they were stored locally. It functions similarly to other cloud storage services like Dropbox or Google Drive, providing seamless access to cloud-stored documents and files through a familiar file explorer interface.
















