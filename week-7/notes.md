**AWS EC2 instance**

Added MFA authentication for my IAM user sara.sipovac
Added custom permission policies for Tier2 called Tier2CreateEC2InstanceUS1 and Tier2SelfManageIamUserPolicy

Inside EC2 service, created security group **sec-web-server-group** that allow SSH and HTTP protocols
on ports 22 and 80 respectively, from all IPv4 addresses

Created EC2 instance **named c2-sara-sipovac-web-server** in eu-central-1 region with:
Amazon Linux 2023 AMI - ami-0fa03365cde71e0ab
t2.micro  instance type
EBS of 14GiB
Key pair sara-sipovac-web-server-key
Security group: sec-web-server-group

Created Biling Alarm **cw-cost-alert-sara-sipovac** inside us-east-1 region:
Metrics - estimated charges
Statistics - maximum
Period - 6hours (because statistics is maximum)
Treshold - >5$

For Biling Alarm it is necessary to create SNS service which receives alerts from Biling Alarm
and sends email to subscriptions.
So I created SNS topic **Default_CloudWatch_Alarms_Topic** with only mine e-mail subscribed.


