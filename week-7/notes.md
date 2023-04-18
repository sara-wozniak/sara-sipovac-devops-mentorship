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

Now, it is needed to connect to EC2 instance using sara-sipovac-web-server-key.pem key-pair from my local machine.
Through cmd I switched to location of key-pair file and run from there ssh -i sara-sipovac-web-server-key.pem ec2user@publicEC2IPAddress.

Then switched to root user with **sudo su -u**

Install NGINX: yum install nginx -y
Start NGINX: systemctl start nginx
This enabled web server on my EC2 instance and if I visit my EC2's public address from browser I can see NGINX default home page.
Further, I need to set up automatic NGINX start with command: **systemclt enable nginx**.
In order to test if it works I will Stop my EC2 instance from AWS console, check in browser - it's not available.
Start EC2 instance, wait to switch from pending to running state, copy new public address into browser and confirm automatic NGINX start,
i.e. no need to SSH to EC2 and run systemctl start nginx.

Install nodejs:
Due to failure of first recommended command **curl _l -o nodesource_setup.sh https://rpm.nodesource.com/setup_14.x**
I have user nvm.
So, first install nvm  with command **curl -o -https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash**".
Install node 14 with command **nvm install 14**.
Check node version with **node --version** which results with **v14.21.3**.

Install git **yum install git -y** (I am  already root user).
In order to fetch application git repo I need to generate ssh key with command **ssh-keygen -t rsa** and choose default configs.
 This generated id_rsa and id_rsa.pub in ~/root/.ssh directory.
I need to copy public key and add it to my GitHub profile via Settings/SSH and GPG keys.
There I created ec2-instanca-git SSH key.

After that I copy SSH link from package I want to install on my machine. So from EC2 terminal I run **git clone SSH_link**.
I have created app inside ~/root directory.
Then I swtich to cd nodejs-simple-app directory and run **npm install** in order to get all libraries that this app depends on,
which is defined in ./package.json file.
Then install globally pm2 **npm install -g pm2**.
List running processes: **pm2 list**.
Start application **pm2 start server.js**.
Run **pm2 startup** and **pm2 save** to create enable running pm2 with server boot.

Configure NGINX:
Inside /ect/nginx/conf.d directory create  simple-node-app.conf file with following content:
server {
listen 80;
server_name 3.68.91.255;

location / {
proxy_pass http://127.0.0.1:8008;
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection 'upgrade';
proxy_set_header Host $host;
proxy_cache_bypass $http_upgrade;
}
}
With location / all endpoints will be forwarded to proxy server i.e. NodeJS application.
This tells NGINX server to listen for traffic on server with EC2's public IP on port 80.
In order to run some app on NGINX it should be configured as reverse proxy meaning that will
forward requests to other application. In this case it is application running on
localhost on port 8008, defined in server.js file of node-simple-app. Other fields define
HTTP version and some header fields.





