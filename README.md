
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
