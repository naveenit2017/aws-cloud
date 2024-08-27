
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













