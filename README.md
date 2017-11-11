Terraform:

1- Create a VPC With 1 Public Subnet and 2 Private Subnet:
Step 1. Select VPC with public and Private Subnets.Now click on Select.
 
Step 2. Now specify following settings as per your requirements as for now i am specified the settings below.
VPC name : Specify any name for your vpc Like: MyFirstPrivateVpc
Availability zone : Specify Availablity as per your requirements
Public Subnet IP: 10.0.0.0/24
Private subnet IP: 10.0.1.0/24
 
Note: Now at the right most side of the page there is a option use a NAT instance instead click on it & specify the below settings.
Instance type : t2.micro
Key pair name : Specify your key pair name
 
Note : Leave rest of the settings default and click on Create VPC.
Step 3. Now your VPC is successfully created.Click on ok to view your vpc.
 
Step 4. Now your VPC is successfully created and in available state.
 
Now Your New VPC created two subnets inside the newly created VPC (MyFirstPrivateVpc):
1.	A public subnet of size /24 (example CIDR: 10.0.0.0/24). This provides 256 (actually 251) IP addresses.
2.	A private subnet of size /24 (example CIDR: 10.0.1.0/24). This provides 256 (actually 251) IP addresses.






Setup 3 Webserver "Nginx" Instances in Different AZ with 1 ELB:
                                                         Or
Install NGINX Web Server on Ubuntu Instance with AWS ELB:
We are general steps for Install NGINX Web Server on Ubuntu Instance with AWS ELB.
1.	On the AWS home page, sign in to the (AWS Management) Console or create a new account.
2.	Click  Services  in the top Console navigation bar, then EC2 in the Compute section.
 
3.	Click the  Launch Instance  button on the page that opens.
4.	On the Step 1 page, click the  Select  button In the Ubuntu Server 16.04 LTS (HVM), SSD Volume Type row.
5.	On the Step 2 page, select the t2.micro instance type, 
 
6.	By default EC2 instances accept only SSH traffic. To allow incoming HTTP and HTTPS traffic:
a.	At the top of the window, click 6. Configure Security Group.
 
b.	On the Step 6 page, click the  Add Rule  button below the table, then select HTTP in the Type column. We are allowing access to our instances from any IP address (the value in the Source column is Anywhere).
c.	Repeat Step b for HTTPS.
7.	Click the  Review and Launch  button at the bottom of the page.
8.	On the Step 7 page, click the  Launch  button at the bottom.
9.	In the box that pops up, create a new key pair:
a.	Select Create a new key pair from the upper drop down menu. 
b.	Type a name in the Key pair name field, such as NGINX_key.
 
c.	Click the  Download Key Pair  button.
d.	In your file manager utility, move the downloaded .pem file to a secure location. For the tutorial we’re placing it in the /Desktop/NGINX directory.
e.	Click the  Launch Instances  button.
10.	On the Launch Status page that opens, click the  View Instances  button at the bottom.
11.	On the page that opens, the new instance appears in the table. Give it a name:
a.	Click the pencil icon in the Name column.
b.	Type the name in the field and click the check mark icon.
12.	Click the  Connect button at the top of the screen. A window like the following pops up.
 
13.	Follow the directions on the pop up to finish connecting to your instance (including the ones accessible by clicking connect using PuTTY, if you’re using Windows). This includes pasting the sample ssh command into your terminal once you’ve navigated to your key.
Installing NGINX Software
Now that you have your AWS virtual machine (EC2 instance) set up, it’s time to install either the open source NGINX software or NGINX Plus, which is available for free in a 30 day trial). Both options work in the context of this tutorial, but if you want to further explore the advanced features in NGINX Plus, please request a free trial.
Installing Open Source NGINX
To install the open source NGINX software, follow these steps:
1.	Access your terminal.
2.	Download the NGINX signing key:
sudo wget http://nginx.org/keys/nginx_signing.key
3.	Add the key:
sudo apt-key add nginx_signing.key
4.	Change directory to /etc/apt.
cd /etc/apt
5.	Edit the sources.list file, appending this text at the end:
6.	deb http://nginx.org/packages/ubuntu xenial nginx
deb-src http://nginx.org/packages/ubuntu xenial nginx
7.	Update the NGINX software:
sudo apt-get update
8.	Install NGINX:
sudo apt-get install nginx
9.	Type Y when prompted.
10.	Start NGINX:
sudo service nginx start
11.	Continue to Opening Your Web Page.
Installing NGINX Plus
1.	If you don’t already have NGINX Plus, then sign up for a 30 day free trial.
2.	When you receive notification that your subscription is available, log in at the NGINX Plus Customer Portal. Access the installation instructions by clicking either of the hyperlinks shown in the screenshot.
 
3.	When installation is complete and NGINX Plus is running, continue to Opening Your Web Page.
Opening Your Web Page
Now that we’ve started the NGINX software, we’ll look at the web page that NGINX and NGINX Plus serve by default before you configure it to deliver your site’s content. Follow these steps: 
1.	Navigate to the Instances tab on the EC2 Dashboard if you are not there already (click Instances in the left hand column.)
 
2.	Select the instance’s IP address in the Public DNS Address column and copy it into the paste buffer.
3.	Open a new tab in the browser and paste the address into the address bar. This appears in the window:
 
Setting Up Sample Files
Now that we know we have a working version of NGINX or NGINX Plus installed, it’s time to put it to good use! Let’s begin by setting up some files and directories.
1.	Change directory to your home directory if you are not already there. In the following instructions, it is /home/ubuntu.
2.	Create a folder called public_html and change into it.
3.	Inside the new folder, create a file called index.html and a folder called application1.
4.	In the application1 folder, create a file called app1.html with some text in it.
5.	Change back to your home directory.
6.	Create a folder called data.
7.	Within the data folder, create a folder called images.
Serving Pages and Images
Our first use case for NGINX or NGINX Plus is to serve pages and images to the user via our web page.
1.	Change directory to /etc/nginx/conf.d.
2.	Rename default.conf to default.conf.bak to prevent NGINX or NGINX Plus from using it as the default configuration file.
3.	Create a file called server1.conf with this configuration in it:
4.	server {
    root /home/ubuntu/public_html;

    location /application1 { }

    location /images {
        root /home/ubuntu/data;
    }
}
5.	Change directory to ~/data/images.

6.	Find an image that you want to serve and copy it to the ~/data/images directory. As an example, the following command copies in the NGINX logo:
sudo wget https://cdn.wp.nginx.com/wp-content/themes/nginx-theme/assets/img//logo.png
7.	Reload NGINX or NGINX Plus:
sudo nginx -s reload
8.	Open your web page and request the image at this URL:
https://NGINX-server/images/logo.png
9.	Also access the application and observe what you get:
https://NGINX-server/application1/app1.html 
Setting Up a Proxy Server
Now that we have a working web server, it’s time to learn how to configure it to route traffic. This capability enables you to pass traffic through to other servers and is a major step towards setting up load balancing. Follow these steps:
1.	In the ~/data folder, create a folder called server2.
2.	In the server2 folder, create a folder called sampleApp.
3.	Inside the sampleApp folder, create a file named index.html and write some text in it.
4.	Change directory to /etc/nginx/conf.d.
5.	Create a file called server2.conf with this configuration in it:
6.	server {
7.	        listen 8080;
8.	        root /home/ubuntu/data/server2;
}
9.	Modify server1.conf as follows:
10.	server {
    root /home/ubuntu/public_html;

    location /application1 {
        proxy_pass http://localhost:8080/sampleApp;
    }
                location /images {
        root /home/ubuntu/data;
    }
}
11.	Reload NGINX or NGINX Plus:
sudo nginx -s reload
12.	Access this URL in your browser and observe what has changed compared to when you accessed it in Serving Pages and Images:
https://NGINX-server/application1/index.html 
So that’s it! You now have a working Ubuntu instance running NGINX, which is ready to run as a proxy server. 


Setup a bastion host for Windows in AWS:
First, set up an internal CNAME reference for the RDG and a an external A record for the same instance using the EIP for the instance.
 
                          An EC2 instance with both an internal and external DNS entry (click to enlarge)
Here you see that I use the subdomain “internal” as a convention to refer to an instance by its VPC private address.
Next, set up the RDP connection. Here, I am using Devolutions’ Remote Desktop Manager which is an excellent RDP client.
 
Specify the internal DNS name as the target RD (click to enlarge) 
Finally, in the RDG settings, use the external name (that is, the A record) name of the RD gateway server.
 
Use the publicly resolvable name of the RD gateway to connect to that gateway (click to enlarge) 
To prove this all works, remove port 3389 from your VPC security group and connect. You can verify that you connected from an external address to the internal address of the RDG in the Windows event log.
 

Configure Remote Desktop Gateway bastion hosts with PowerShell:
The general steps are given below:
<#
    .SYNOPSIS
        Configure a Remote Desktop Gateway jump (bastion) server
    
    .DESCRIPTION
        Uses the Remote Desktop Services PowerShell provider to create/install an RD-CAP and an RD-RAP. Also creates and exports a self-signed certificate for use on connecting clients
    
    .PARAMETER dnsName
        FQDN of the to-be-generated self-signed certificate
    
    .OUTPUTS
        Self-signed cert at $HOME/desktop/$dnsName.cert 
    
    .NOTES
        Remote Desktop Service role must exist on server before this script is run.
        This script adds non-AD local groups to RD-CAP and permits all accesses to back-end resources
      
        Alex Neihaus 2017-07-25
        (c) 2017 Air11 Technology LLC -- licensed under the Apache OpenSource 2.0 license, https://opensource.org/licenses/Apache-2.0
        Licensed under the Apache License, Version 2.0 (the "License");
        you may not use this file except in compliance with the License.
        You may obtain a copy of the License at
        http://www.apache.org/licenses/LICENSE-2.0
        Unless required by applicable law or agreed to in writing, software
        distributed under the License is distributed on an "AS IS" BASIS,
        WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
        See the License for the specific language governing permissions and
        limitations under the License.
        Author's blog: https://www.yobyot.com
    
#>
 
Import-Module RemoteDesktopServices
# Create a self-signed certificate. This MUST be installed in the client's Trusted Root store for RDP clients to be able to use it
$dnsName = "yourvmname.eastus.cloudapp.azure.com"
$x509Obj = New-SelfSignedCertificate -CertStoreLocation Cert:\LocalMachine\My -DnsName $dnsName
# Export the cert to the administrator's desktop for use on clients
$x509Obj | Export-Certificate -FilePath "C:\$HOME\$dnsName.cer" -Force -Type CERT
# See https://blogs.technet.microsoft.com/ptsblog/2011/12/09/extending-remote-desktop-services-using-powershell-part-5/
#   for details of using the RDS provider. It's very poorly documented. If you need to find additional items, sl to the GatewayServer location you are interested in
#   and gci . -recurse | fl
# Create RD-CAP with two user groups; defaults permit all device redirection. Might be worth tightening up in terms of security.
$capName = "RD-CAP-$(Get-Date -Format FileDateTimeUniversal)"
Set-Location RDS:\GatewayServer\SSLCertificate #Change to location where self-signed certificate is specified
Set-Item .\Thumbprint -Value $x509obj.Thumbprint # Update RDG with the thumprint of the self-signed cert.
# Create a new Connection Authorization Profile
New-Item -Path RDS:\GatewayServer\CAP -Name $capName -UserGroups @("administrators@BUILTIN"; "Remote Desktop Users@BUILTIN") -AuthMethod 1
# Create a new Resouce Authorization Profile with "ComputerGroupType" set to 2 to permit connections to any device
$rapName = "RD-RAP-$(Get-Date -Format FileDateTimeUniversal)"
New-Item -Path RDS:\GatewayServer\RAP -Name $rapName -UserGroups @("administrators@BUILTIN"; "Remote Desktop Users@BUILTIN") -ComputerGroupType 2
Restart-Service TSGateway # We're done; let's put everything into effect

Creating a Bastion Host for AWS:
Bastion Host: A special purpose computer on a network specifically designed and configured to withstand attacks. In the context of SSH for AWS, a bastion host is a server instance itself that you must SSH into before you are able to SSH into any of the other servers in your VPC. 
Here are the steps I took to set one up:
Step 1: Create an EC2 instance on AWS. I choose a micro sized instance since it is on the free-tier and it’s only purpose is to access other servers.
Step 2: Create a security group for the Bastion host that opens up port 22 for SSH and select “My IP” as the source. This is so that only you and other teammates that add their IP’s have access to the Bastion. 
Step 3: Change the security groups of existing instances so that any inbound SSH is only accessible via the Bastion host’s IP address. 
Step 4: Edit your local ~/.ssh/config file and add the following: 

Host bastion

Hostname 11.111.111.11

User username

ForwardAgent yes
Where hostname is the IP address of the bastion host and username is the one that use to log into the server. 
This allows you to ssh into your Bastion server by just typing in ssh bastion from the command line. Also, you can see that there is a line above that says Forward Agent yes . 
This sets up SSH forwarding from your local machine to the Bastion Host so that the .pem file you use to access your EC2 instances will be made available when you try to connect to your servers.
Step 5: SSH into the Bastion Host and then try to SSH into any of your existing AWS server instances and voilà! You now have a more secure way of getting into your servers because they are now only accessible from the Bastion. 
Sources: I used this guide here for help on best practices for a SSH Bastion Host and could be useful for those setting up ssh-agent on a Mac or Windows machine.

DNS Using Route 53:

Step 4. Now Click on Create Hosted Zones again which is available at the top of the page
 
Step 5. Now enter Domain name Which you want to host in Domain Name box & choose type Public Hosted Zone.Now click on create.
 
Step 6. By default it will create two Record Set automatically.We can also create more Record Sets as per our requirements.
 
Step 7. Click on Create Record Set to create your new record set.Now on the right most side of the page specify the name type and value as per your requirements
 
Step 8. Now click on Create.After click on create  your record set will add in Hosted Zone and look like below image.
 
Step 9. Now under the Record Set does not mention anything under the name.
 
Note: Reason behind is your website will be open form simply by just typing your domain name without using www.
Step 10. Now click on Create.After click on create it your record set will add in hosted zone and look like below image.
 
Step 11. If you want to check your domain name is working properly then copy your domain and paste onto the web browser and run it.






Route53 weighted routing:
Everything is the same as before (we will create www.vtep.net) up to the point where the resource sets have to be configured. The IP addresses used are the same. The difference will come from the “Routing Policy.” In this case, we will select “Weighted.” Then select the value for “Weight.” Because we want to have 50% through EU CENTRAL region, we will use 50. For “Set ID,” use something that is meaningful for you to identify the record set:
 
The same for the second record set, except that you will use the IP address from the EC2 instance from the US EAST region:
 
Because it’s hard to see the distribution in the Route 53 responses, we will use a script that will check 1000 times (one check per second) what is the resource record for the www.vtep.net. The script is using the Linux command “dig” to get this information and I save only the relevant content: the IP address returned when the command was issued and how long it took to get the answer.

The script is returning something like this:
www.vtep.net. 34 IN A 54.93.72.190
;; WHEN: Tue Jan 13 12:43:35 2015
www.vtep.net. 34 IN A 54.174.204.200
;; WHEN: Tue Jan 13 12:43:37 2015
www.vtep.net. 14 IN A 54.174.204.200
;; WHEN: Tue Jan 13 12:43:38 2015
www.vtep.net. 29 IN A 54.93.72.190
;; WHEN: Tue Jan 13 12:43:39 2015
Let’s check how many entries we have for 54.93.72.190, which is the IP from EU CENTRAL and how many for 54.174.204.200 which is the IP from US EAST:
[UBUNTU:/] access% cat output | grep 190 | wc -l
512
[UBUNTU:/] access% cat output | grep 200 | wc -l
488
[UBUNTU:/] access%
As you can see, it’s almost a perfect even distribution: 51.2% and 48.8%. In time, this will tend to be closer to 50/50.
And our weighted routing policy worked.
And that’s all with latency and weighted routing policies in Route 53.
We have reached the end of the article and the Route 53 series.
In this article you found out what latency and weighted routing policies are and how you can configure them.
Now, by going through all three parts of the Route 53 series, you should be familiar with the service. Now that the foundation is there, you can go and explore the mode advanced features that Route 53 provides.



Website healthcheck with Cloudwatch:
CloudWatch does much of the heavy lifting for this project. If you are unfamiliar with the functionality, I suggest reading up on the concepts. 
This project will be two pieces:
1.	A CloudFormation template to create the SNS topic and CloudWatch Alarms 
2.	A Serverless Project that creates the Lambda functions and CloudWatch Events.
Resources
Let's start by creating a CloudFormation template for the SNS Topic and the Alarms.
AWSTemplateFormatVersion: "2010-09-09"  
Description: Template for deploying the website Health Checks

Resources:

  alert:
    Type: AWS::SNS::Topic
    Properties:
      DisplayName: Website Uptime
      Subscription:
        -
          Endpoint: 14125550100
          Protocol: sms

  homeAlarm:
    Type: AWS::CloudWatch::Alarm
    Properties:
      ActionsEnabled: true
      AlarmActions:
        - !Ref alert
      AlarmName: homeAlarm
      ComparisonOperator: GreaterThanOrEqualToThreshold
      EvaluationPeriods: 1
      MetricName: home-status
      Namespace: Website
      Period: 60
      Statistic: Average
      Threshold: 500
The first resource is for a AWS::SNS::Topic. This topic is defined so that it sends an SMS message to the supplied phone number. Don't forget the country code!
The second resource is the AWS::CloudWatch::Alarm. This alarm is configured to trigger the SNS alert when an alarm is triggered. We'll also preemptively have it track the Website namespace and the metric home-status. This alarm will look for the average value of the metric over 60 seconds and trigger the alarm if it is >= 500 for more than 1 period. This means if we see 500 errors for a minute, the alarm will be triggered and the SNS alert will be published.
This can be deployed by running the CloudFormation AWS CLI command:
aws cloudformation create-stack \  
  --stack-name ssc2-healthcheck \
  --template-body file://cloudformation.yml
Serverless App
The next piece is using Serverless to create a Lambda function that will trigger an HTTP request and report that value to CloudWatch Metrics. Further, this Lambda will be configured with CloudWatch Events to trigger every 1 minute!
Let's start with the serverless.yml definition.
service: website-healthcheck

frameworkVersion: '1.13'

provider:  
  name: aws
  runtime: nodejs6.10

  stage: production
  region: us-east-1

  iamRoleStatements:
    - Effect: Allow
      Action:
        - "cloudwatch:*"
      Resource:
        - "*"

functions:  
  checkHome:
    handler: handler.checkHome
    events:
      -
        schedule:
          name: check-home
          rate: rate(1 minute)
Of note here is the iamRoleStatements. We need to grant the Lambda function access to write the CloudWatch Metrics. 
Beyond that, the actual function will be called checkHome and will have a scheduled event that gets triggered every minute.
Now to the last piece, the actual lambda function. In handler.js, create our function as such:
let http       = require('http');  
let AWS        = require('aws-sdk');  
let cloudwatch = new AWS.CloudWatch();

module.exports = {  
  checkHome,
};

function checkHome(event, context, callback) {  
  checkStatus({ host: 'www.example.com', path: '/' })
    .then(putMetric)
    .then(() => callback(null, { success: true }))
    .catch(callback);
}

//////////////////////////////////////

/**
 * Makes an HTTP request and resolves with 
 * the HTTP status code 
 */ 
function checkStatus({ host, path }) {  
  return new Promise((resolve, reject) => {
    let req = http.request({ host, path }, (res) => {
      res.on('error', reject);
      resolve(res.statusCode);
    });
    req.on('error', reject);
    req.end();
  });
}

/**
 * Puts the CloudWatch metric in the Website namespace
 * as the home-status metric.
 */
function putMetric(statusCode) {  
  let params = {
    Namespace: 'Website',
    MetricData: [
      {
        MetricName: 'home-status',
        Timestamp: new Date(),
        Value: statusCode
      }
    ]
  };
  return cloudwatch.putMetricData(params).promise();
}
We export a single function called checkHome. This corresponds to the serverless.yml file above. This function will first make an HTTP request and will then put the CloudWatch Metric using the AWS SDK.
You can then deploy your serverless function with
serverless deploy  
Things should now be running! You can log into your CloudWatch dashboard and check out your alerts.





CPU utilization Health Check with Cloudwatch:
The general steps are given below:
STEP 1. Click On CloudWatch In AWS Management Console.
 
STEP 2. One the left most part of webpage Click on Alarm.
 
STEP 3.Click on Create Alarm.
 
STEP 4. Select  EC2 Metric  and also select metric name which you want to monitor. I choose CPU utilization. After that click on Next.
 
STEP 5. Specify the Name and Description of Alarm and also specify the CPU Utilization Percent according to your need and if you want to receive alerts on your Mail ID than specify the Mail ID in Notification Box. After that click on Create Alarm  
 
Note: After click on Create Alarm. AWS will send a confirmation link on your Mail ID. For further proceed you have to confirm that link on your Mail ID.
STEP 6. After successfully completing all steps your Alarm will created with OK state.
 
 Now, CPU utilization Health Check on cloudwatch dashboards.
 Configuration SES (Simple Email Service):
The general steps are given below:
Step 1. Click on Email addresses option under the service panel of the page.

Step 2. Click on Verify a New Email Address button on the top of the page & specify the Email Address in Email Address box which you want to verify.
 
 
Now click on Verify This Email Address button.
Step 3. In your email client, open the message from Amazon SES asking you to confirm that you are the owner of this email address.Click on Link Message on your email client, after that status of the email address in the Amazon SES console will change from pending verification to verified.
Step 4. Click on SMTP settings option under the service panel of the page.

Step 5. Click on Create My SMTP Credentials button to obtain SMTP username and password that you use to connect the Amazon SES SMTP endpoint.
 
Step 6. Enter the name of a new IAM user or accept the default as per your requirements.After that click on Create button to set up your SMTP credentials.

Step 7. Your SMTP credentials are generated successfully.Click on Download Credentials button at the bottom of the page to download credentials in your local machine.
 
Step 8. Your Amazon Simple Email Service account has a set of sending quota to regulate the number of email messages that you can send.You have the option to increase a sending quota limit.For this click on sending statistics option under the service panel of the page.
 
Step 9. Now at the top of the page, there is a big blue button Request a sending Limit increase button.Click on it.
 
Step 10. Specify the following details:
Regarding              –    Service Limit Increase
Limit Type             –    SES Sending Limits
Region                    –     Specify region as per your requirements
Limit                        –    Desired Daily Sending Quota
New limit value   –    As per your need
Mail type                –    System Notifications
 
After specifying all the details click on submit button to submit your Case.
Now email notification services work done.











Create an docker image based on ubuntu 16.04 for PHP/NGINX App:
Setting up Nginx
We’ll start by getting ourselves a web server and based on our requirements this will be a container running the official Nginx image. Since we’ll be using Docker Compose, we will create the following docker-compose.yml file, which will run the latest Nginx image and will expose its port 80 to port 8080:
web:
 image: nginx:latest
 ports:
 - "8080:80"
Now we can run
docker-compose up
This should give you the default Nginx screen on port 8080 for localhost or the IP of your docker machine.
 
Now that we have a server let’s add some code. First we have to update the docker-compose.yml to mount a local directory. I will use a folder called code, which is in the same directory as my docker-compose.yml file, and it will be mounted as root folder code in the container.
web:
    image: nginx:latest
    ports:
        - "8080:80"
    volumes:
        - ./code:/code
The next step is to let Nginx know that this folder exists.
Let’s create the following site.conf on the same level as the docker-compose.yml file:
server {
    index index.html;
    server_name php-docker.local;
    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;
    root /code;
}
If you don’t have a lot of experience with Nginx, this is what we define here – index.html will be our default index, the server name is php-docker.local and it should be pointing (update your hosts file) to your Docker environment (localhost if you are on Linux or the docker machine if you are on Mac or Windows), we point the error logs to be the ones exposed by the default container, so that we will see the errors in our docker compose log, and finally we specify the root folder to be the one that we mounted in the container.
To activate this setup we need to apply yet another modification to our docker-compose.yml file:
web:
    image: nginx:latest
    ports:
        - "8080:80"
    volumes:
        - ./code:/code
        - ./site.conf:/etc/nginx/conf.d/site.conf
This will add site.conf to the directory where Nginx is looking for configuration files to include. You can now place an index.html file in the code folder with contents that is to your heart’s delight. And if we run
docker-compose up
again, the index.html file should be available on php-docker.local:8080.
Yeey! We are half way there

Adding PHP-FPM
Now that we have Nginx up and running let’s add the PHP in the game. The first thing we’ll do is pull the official PHP7-FPM repo and link it to our Nginx container. Our docker-compose.yml will look like this now:
web:
    image: nginx:latest
    ports:
        - "8080:80"
    volumes:
        - ./code:/code
        - ./site.conf:/etc/nginx/conf.d/site.conf
    links:
        - php
php:
    image: php:7-fpm
The next thing to do is configure Nginx to use the PHP-FPM container for interpreting PHP files. Your updated site.conf should look like this:
server {
    index index.php index.html;
    server_name php-docker.local;
    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;
    root /code;

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass php:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }
}
In order to test this let’s rename the index.html file to index.php and replace its content with the standard:
<?php
echo phpinfo();
One final
docker-compose up
And we should be good to go... but

Instead of getting the proper PHP info page we receive the rather unsettling
File not found.
Since PHP is running in its own environment (container) it doesn't have access to the code. In order to fix this, we need to mount the code folder in the PHP container too. This way Nginx will be able to serve any static files, and PHP will be able to find the files it has to interpret. One final change to the docker-compose.yml:
web:
    image: nginx:latest
    ports:
        - "8080:80"
    volumes:
        - ./code:/code
        - ./site.conf:/etc/nginx/conf.d/site.conf
    links:
        - php
php:
    image: php:7-fpm
    volumes:
        - ./code:/code
Finally, this last (this time for real)
docker-compose up
present us with the much wanted PHP info

 
This is it.






Create Ubuntu Docker image on MongoDB :
The general steps are given below:
# Dockerizing MongoDB: Dockerfile for building MongoDB images

# Based on ubuntu:16.04, installs MongoDB following the instructions from:

# http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/



FROM ubuntu:16.04

MAINTAINER Docker - Pablo Ezequiel



# Installation:

# Import MongoDB public GPG key AND create a MongoDB list file

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927

RUN echo "deb http://repo.mongodb.org/apt/ubuntu $(cat /etc/lsb-release | grep DISTRIB_CODENAME | cut -d= -f2)/mongodb-org/3.2 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.2.list



# Update apt-get sources AND install MongoDB

RUN apt-get update && apt-get install -y mongodb-org



# Create the MongoDB data directory

RUN mkdir -p /data/db



# Expose port #27017 from the container to the host

EXPOSE 27017



# Set /usr/bin/mongod as the dockerized entry-point application

ENTRYPOINT ["/usr/bin/mongod"]








Generating and Install SSH Keys on Ubuntu:

Method -1st:
OpenSSH server supports various authentication schema. The two most popular are as follows:
1.	Passwords based authentication:

2.	Public key based authentication. It is an alternative security method to using passwords. This method is recommended on a VPS, cloud, dedicated or even home based server.
The first step in the installation process is to create the key pair on the client machine, which would, more often than not, be your own system. Users need to use the following command:
ssh-keygen -t rsa
The above command kicks off the SSH Key installation process for users. 
Step Two-Storing the Keys and Passphrasing
Upon entering the primary Gen Key command, users need to go through the following drill by answering the following prompts:
Enter the file where you wish to save the key (/home/demo/.ssh/id_rsa) 
Users need to press ENTER in order to save the file to the user home 
The next prompt would read as follows: 
Enter passphrase
If, as an administrator, you wish to assign the passphrase, you may do so when prompted (as per the question above), though this is optional, and you may leave the field vacant in case you do not wish to assign a passphrase. 
However, it is pertinent to note there that keying in a unique passphrase does offer a bevy of benefits listed below: 




Here is a broad outline of the end-to-end key generation process: 
root@server1:~# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
38:16:50:fe:8d:da:02:bb:46:1b:66:0c:10:8e:81:2d root@server1.example.com
The key's randomart image is:
+--[ RSA 2048]----+
|+o ... |
|E.. o |
|.+ o |
| . + o |
| o. + S . |
| *+ + |
| +.oo . |
| o. . |
| .. |
+-----------------+ 
The public key can now be traced to the link ~/.ssh/id_rsa.pub 
The private key (identification) can now be traced to the link-/home/demo/.ssh/id_rsa 3
Step Three-Copying the Public Key
________________________________________ 
Once the distinct key pair has been generated, the next step remains to place the public key on the virtual server that we intend to use. Users would be able to copy the public key into the authorized_keys file of the new machine using the ssh-copy-id command. Given below is the prescribed format (strictly an example) for keying in the username and IP address, and must be replaced with actual system values: 
ssh-copy-id user@192.168.0.100
As an alternative, users may paste the keys by using SSH (as per the given command): 
cat ~/.ssh/id_rsa.pub | ssh user@192.168.0.100 "cat >> ~/.ssh/authorized_keys"
Either of the above commands, when used, shall toss the following message on your system: 
The authenticity of host '192.168.0.100 ' can't be established. RSA key fingerprint is b1:2d:32:67:ce:35:4d:5f:13:a8:cd:c0:c4:48:86:12. Are you sure you want to continue connecting (yes/no)? yes Warning: Permanently added '192.168.0.100' (RSA) to the list of known hosts. user@192.168.0.100's password: Now try logging into the machine, with "ssh 'user@192.168.0.100'", and check in: ~/.ssh/authorized_keys to make sure we haven't added extra keys that you weren't expecting. 
After the above drill, users are ready to go ahead and log into user@192.168.0.100 without being prompted for a password. However, if you have earlier assigned a passphrase to the key (as per Step 2 above), you will be prompted to enter the passphrase at this point (and each time for subsequent log-ins.). 
Step Four (This Step is Optional)-Disabling the Password to Facilitate Root Login
After users have copied their SSH keys unto your server and ensured seamless log-in with the SSH keys only, they have the option to restrict the root login, and permit the same only through SSH keys. To accomplish this, users need to access the SSH configuration file using the following command: 
sudo nano /etc/ssh/sshd_config
Once the file is accessed, users need to find the line within the file that includes PermitRootLogin , and modify the same to ensure a foolproof connection using the SSH key. The following command shall help you do that: 
PermitRootLogin without-password
The last step in the process remains to implement the changes by using the following command: 
reload ssh
The above completes the process of installing SSH keys on the Linux server. 
Method -2nd: Generating SSH Keys for windows, Linux, and Apple’s OS X:
The general steps are given below:
1.	To generate SSH keys for your host, issue the following command on your local system:
ssh-keygen

2.	Optional: to increase the security of your key, increase the size with the -b flag. The minimum value is 768 bytes and the default, if you do not use the flag, is 2048 bytes. We recommend a 4096 byte key:
ssh-keygen -b 4096
3.	Answer all questions when prompted. You can accept the defaults for everything except the passphrase. When you get to the passphrase question, enter a series of letters and numbers for the passphrase twice; once to enter the new passphrase and once to confirm.




Important: make a note of your passphrase, as you will need it later.
You may accept the defaults for the other questions by pressing Return when prompted:
1
2
3
4
5
6
7
8
9	user@linode: ssh-keygen -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/user/.ssh/id_rsa.
Your public key has been saved in /home/user/.ssh/id_rsa.pub.
The key fingerprint is:
f6:61:a8:27:35:cf:4c:6d:13:22:70:cf:4c:c8:a0:23 user@linode

The newly-generated SSH keys are located in the ~/.ssh/ directory. You will find the private key in the ~/.ssh/id_rsa file and the public key in the ~/.ssh/id_rsa.pub file.
Uploading Keys
Please note that the following steps are performed on your remote location.
1.	Before you upload the keys, verify that your .ssh directory exists by using the following command from your home directory (the default directory when you log in):
1	ls -al

2.	If the .ssh directory is present, proceed to Step 3. If the directory is not present, issue the following command in the /home/user directory to create it:
1	mkdir .ssh

3.	The following steps are performed on your local machine/PC:
4.	Copy the public key into the ~/.ssh/authorized_keys file on the remote machine, using the following command. Substitute your own SSH user and host names:
1	scp ~/.ssh/id_rsa.pub user@example.com:/home/user/.ssh/uploaded_key.pub

5.	Run the following command to copy the key to the authorized_keys file. Substitute your own SSH user and host names:
1	ssh user@example.com "echo `cat ~/.ssh/uploaded_key.pub` >> ~/.ssh/authorized_keys"
Connecting to the Remote Server
The final part in the SSH key process is to access your Remote Server with your new private key.
1.	Connect to the remote server.
2.	Depending on your desktop environment, a window may appear prompting you for a password. Otherwise, you will be prompted in your terminal. This password is the passphrase you created for the private key encryption.
  
3.	If you’re on a private computer, you can check the Remember password in my keychain box to save your passphrase. If you are logged on via a public machine, don’t check this box, as this would compromise your security and allow access to your Remote Server.
4.	Click the OK button.
You should now be connected to your Remote Server using the SSH key.
add www-data user:
Add user to www-data group by following command:
1.Add user to www-data group:
sudo adduser username www-data

2. Add /var/www to www-data (recursive):
sudo chown -R www-data:www-data /var/www

3. Change file and folder permissions on /var/www (recursive):
sudo chmod -R g+rw /var/www

Remote host login using SSH key without a password:
Step One—Create the RSA Key Pair
The first step is to create the key pair on the client machine (there is a good chance that this will just be your computer):
ssh-keygen -t rsa
Step Two—Store the Keys and Passphrase
Once you have entered the Gen Key command, you will get a few more questions:
Enter file in which to save the key (/home/demo/.ssh/id_rsa):
You can press enter here, saving the file to the user home (in this case, my example user is called demo).
Enter passphrase (empty for no passphrase):
It's up to you whether you want to use a passphrase. Entering a passphrase does have its benefits: the security of a key, no matter how encrypted, still depends on the fact that it is not visible to anyone else. Should a passphrase-protected private key fall into an unauthorized users possession, they will be unable to log in to its associated accounts until they figure out the passphrase, buying the hacked user some extra time. The only downside, of course, to having a passphrase, is then having to type it in each time you use the Key Pair.
The entire key generation process looks like this:
ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/demo/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/demo/.ssh/id_rsa.
Your public key has been saved in /home/demo/.ssh/id_rsa.pub.
The key fingerprint is:
4a:dd:0a:c6:35:4e:3f:ed:27:38:8c:74:44:4d:93:67 demo@a
The key's randomart image is:
+--[ RSA 2048]----+
|          .oo.   |
|         .  o.E  |
|        + .  o   |
|     . = = .     |
|      = S = .    |
|     o + = +     |
|      . o + o .  |
|           . o   |
|                 |
+-----------------+
The public key is now located in /home/demo/.ssh/id_rsa.pub The private key (identification) is now located in /home/demo/.ssh/id_rsa
Step Three—Copy the Public Key
Once the key pair is generated, it's time to place the public key on the virtual server that we want to use.
You can copy the public key into the new machine's authorized_keys file with the ssh-copy-id command. Make sure to replace the example username and IP address below.
ssh-copy-id user@123.45.56.78
Alternatively, you can paste in the keys using SSH:
cat ~/.ssh/id_rsa.pub | ssh user@123.45.56.78 "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys"
No matter which command you chose, you should see something like:
The authenticity of host '12.34.56.78 (12.34.56.78)' can't be established.
RSA key fingerprint is b1:2d:33:67:ce:35:4d:5f:f3:a8:cd:c0:c4:48:86:12.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '12.34.56.78' (RSA) to the list of known hosts.
user@12.34.56.78's password: 
Now try logging into the machine, with "ssh 'user@12.34.56.78'", and check in:

  ~/.ssh/authorized_keys

to make sure we haven't added extra keys that you weren't expecting.
Now you can go ahead and log into user@12.34.56.78 and you will not be prompted for a password. However, if you set a passphrase, you will be asked to enter the passphrase at that time (and whenever else you log in in the future).
Optional Step Four—Disable the Password for Root Login
Once you have copied your SSH keys unto your server and ensured that you can log in with the SSH keys alone, you can go ahead and restrict the root login to only be permitted via SSH keys.
In order to do this, open up the SSH config file:
sudo nano /etc/ssh/sshd_config
Within that file, find the line that includes PermitRootLogin and modify it to ensure that users can only connect with their SSH key:
PermitRootLogin without-password
Put the changes into effect:
reload ssh


Scripting:

Send a new .pem file via email:
Example: if you want to send an encrypted PEM formatted key(/tmp/PSK.txt) to B:

Note: This work only for a single line formatted in PEM and NOT for a file.
For a file you need to use the second method shown below.
B creates a public key(/tmp/public.pem) in PEM format using his own ssh private-key(~/.ssh/id_rsa) and send it to A via email for example.:

openssl rsa -in ~/.ssh/id_rsa -out /tmp/public.pem -outform PEM –pubout

It would look like this for example:

-----BEGIN PUBLIC KEY-----
MIGdMA0GCSqGSIb3DQEBAQUAA4GLADCBhwKBgQDHxee5n1Na06RdBUjIYMRH5aC5
zo4XvgxC8NED0yUzjfX+adFgQbTRVuNOBtnSVm83XT1Bp/BKQqqx0hcMlazVRjrH
x2jUr4nCczSvVnq4cKzDyvO7tj5kOg1m6fliXcKv4cqjftQpOnz4RTmieyjb3+aN
1/JynAFpnLxKrF8bZQIBIw==
-----END PUBLIC KEY-----

A uses this public key(public.pem) to encrypt the PSK file(/tmp/PSK.txt) and send it to B via email:

openssl rsautl -encrypt -inkey /tmp/public.pem -pubin -in /tmp/PSK.txt -out /tmp/PSK.ssl

B decrypt the ecnryped file using his own ssh private-key(~/.ssh/id_rsa) to a file(/tmp/PSK.txt):

openssl rsautl -decrypt -inkey ~/.ssh/id_rsa -in /tmp/PSK.ssl -out /tmp/PSK.txt
Sending a FILE securely:

A wants to encrypt a file(/tmp/file.txt) and send it to B: 

B creates a self signed certificate using his own ssh private-key(~/.ssh/id_rsa) and sends it to A via email:

openssl req -x509 -new -key ~/.ssh/id_rsa > /tmp/rsa.crt
Country Name (2 letter code) [AU]:DE
State or Province Name (full name) [Some-State]:Berlin
Locality Name (eg, city) []:Berlin
Organization Name (eg, company) [Internet Widgits Pty Ltd]:My Company Ltd
Organizational Unit Name (eg, section) []:Sysadmin
Common Name (e.g. server FQDN or YOUR name) []: MyCompany
Email Address []:sunilcareerplus@myserver.com

A encrypt the file(/tmp/file.txt) with the Certificate(/tmp/rsa.crt) and send it to B via email:

openssl smime -encrypt -binary -aes-256-cbc -in /tmp/file.txt -out /tmp/file.enc -outform DER /tmp/rsa.crt

B decrypt the file using his own ssh private-key(~/.ssh/id_rsa):

openssl smime -decrypt -binary -in /tmp/file.enc -inform DER -out /tmp/file.txt -inkey ~/.ssh/id_rsa

The file(/tmp/file.txt) is then been transfered securely from A(/tmp/file.txt) to B.
Create a .pem File for SSL Certificate Installations:
A .pem file is a container format that may just include the public certificate or the entire certificate chain (private key, public key, root certificates):
•	Private Key 
•	Server Certificate (crt, puplic key) 
•	(optional) Intermediate CA and/or bundles if signed by a 3rd party
Create a self-signed PEM file:
openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.pem -out cert.pem
Create a PEM file from existing certificate files that form a chain:
•	(optional) Remove the password from the Private Key by following the steps listed below:
o	Type openssl rsa -in server.key -out nopassword.key and press Enter. 
o	Enter the pass phrase of the Private Key. 
•	Combine the private key, public certificate and any 3rd party intermediate certificate files: 
o	cat nopassword.key > server.pem 
o	cat server.crt >> server.pem
	Repeat this step as needed for third-party certificate chain files, bundles, etc:
cat intermediate.crt >> server.pem

Generate new autorized_keys:
1. Login to the Host via SSH using your preferred terminal application and generate the public / private key pair. In terminal type the following at the command prompt: 
#    ssh-keygen -t rsa -C "server comment field"
Note: the -C switch is not required. It allows you to insert a comment that will appear in the authorized_keys file. It is helpful for identifying and managing keys within the authorized_keys file on the Client in the event that you have multiple key logins. Replace "server comment field" with a machine name, IP address, date, or task name so that you can easily identify where and why a given key was created. 
2. Execute the command and you should see the following output: 
#   Generating public/private rsa key pair. 
#   Enter file in which to save the key (/Users/UserName/.ssh/id_rsa):
Note: “UserName” is the user account that you have logged into via SSH. Also note that the actual suggested path may vary slightly depending your system. You should accept the suggested location unless you have reason to do otherwise. 
You will then be prompted for a passphrase that will be associated with this key. 
#   Enter passphrase (empty for no passphrase):
#   Enter same passphrase again:
The passphrase can be thought of as a password for the private key - it serves as an extra layer of protection as described below.
3.  The final steps are to copy the public key to the Client and append it to the authorization_keys file. In the terminal navigate to the folder that contains the newly created keys – cd /User/UserName/.ssh and use the "ls" list comand to see what is in that directory. You should see the following files: 
•	id_rsa
•	id_rsa.pub
•	known_hosts.
Type the following at the command prompt: 
#   scp id_rsa.pub admin@ClientIPAddress /etc/config/ssh/
admin@ClientIPAddress is the address of your QNAP NAS (the Client) followed by the path on the Client where the public key needs to reside. You will be ask you for the password of the "admin" account to login to the Client. 
#   password:
Enter the password to complete secure copy. 
4. On the Client (QNAP NAS) navigate to the /etc/config/ssh folder and "ls" to reveal the contents of the directory. You should see your id_rsa.pub file. 
5. Now let's append this file to the authorized_keys file which needs to reside in this directory. Do not worry if authorized_keys file is not present. We will create it. 
Type the following at the command prompt: 
#   cat id_rsa.pub >> authorized_keys
Note: You have to cut the key in the file to one line, and add ssh-rsa in at the very beginning. 
Example 
ssh-rsa AAAAB3NzaC1kc3MAAACBAPY8ZOHY2yFSJA6XYC9HRwNHxaehvx5wOJ0rzZdzoSOXxbETW6ToHv8D1UJ/z+zHo9Fiko5XybZnDIaBDHtblQ+Yp7Stx...4loWgV=
Set correct permissions 
SSH daemon is really peacky about permissions. Create a .ssh folder within your home folder, copy /etc/config/ssh/authorized_keys to this folder and then make sure you have set your permissions as follows: 
chmod 0711 ~
chmod 0700 ~/.ssh
chmod 0600 ~/.ssh/authorized_keys
That's it. You should now be able to login using key authentication. Logout of the Client and attempt to login. If you created a passphrase for your id_rsa private key then you will be prompted for the passphrase. If you left the passphrase field blank when generating the keys then you will be logged in automatically.

The first time you login you may encounter a promoted message like below. 
The authenticity of host 'xxx.xxx.xxx.xxx' can't be established.
RSA key fingerprint is XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX.
Are you sure you want to continue connecting (yes/no) ?
Type "yes" to continue. After you accept the authenticity of the RSA key, the Client information is saved in the /Users/UserName/.ssh/known_hosts file. You will not be prompted again unless you remove this file. 
Important
If the destination Client is a x86 series model, please execute the following command on the destination Client (TS-509) to make sure the folder permission has been set correctly. Because there might be a permission problem in earlier firmware versions in x86 model (e.g. TS-509) 
#   chown admin.administrators /mnt/HDA_ROOT/.config


Add a Deploy User to a Remote Linux Server:
In a later step, you will need your development machine’s (or whatever machine you’re deploying from) public SSH key. Go ahead and copy it down now.
cat ~/.ssh/id_rsa.pub 
Be sure to copy your key exactly. If you add spaces or leave out characters it will not work.
1. Creating the Deploy User
1. To begin, login to the remote system (that is, the system you are deploying to), and become root. Then issue useradd to create the deploy user:
useradd --create-home -s /bin/bash deploy
This will add a new user named deploy, create a home directory for it (/home/deploy), and give it a login shell (/bin/bash).
Next, you need to create a password for the deploy user. If you don’t create a password the account will remain “locked” and you won’t be able to login. Note, however, that the password is never actually used. Since the password is never used for anything, I recommend setting it to a long, random string. If you need a password, here’s one that was randomly generated: 7976dcdf5ebdc77b65f4c86a0e1a3b15002b58efd647228d
2.Use passwd to set a password:
passwd deploy
3. After you’ve set the deploy user’s password, you need to create a directory and file for the authorized SSH keys. The authorized_keys file holds the public keys of the machines you will be deploying from.
mkdir /home/deploy/.ssh
touch /home/deploy/.ssh/authorized_keys

2. Adding Your SSH Key
Assuming that you have disabled password logins for SSH, you won’t be able to use ssh-copy-id to copy your SSH key. As such, you will need to manually add your key (that you copied during the first step).

1.	Open the authorized_keys file you created in the previous step:
                vi /home/deploy/.ssh/authorized_keys
2. Paste your public SSH key exactly as you copied it. If you have multiple keys from multiple machines then paste one key per line.
3. Save and close the authorized_keys file, then chown and chmod it to lock it down:
chown -R deploy. /home/deploy
chmod 600 /home/deploy/.ssh/authorized_keys
3. Test Everything
If all went as planned, you should now be able to login to your remote server using the deploy user. Let’s test and see. Try logging in from your development/deployment machine:
ssh deploy@remote-server.com
You should now be logged into the remote machine as deploy. If you are prompted for a password or receive a Permission denied (publickey) error then it’s likely you copied/pasted your SSH key wrong.
A Note About Permissions
Depending on how your remote server is setup, you may run into permission issues when trying to deploy. This is likely because your web files are owned by a user like www-data, and the deploy user doesn’t have permission to modify them. The easiest way I’ve found around this is to add the deploy user to the www-data group and then chmod the files to allow group access. For example:
usermod -a -G www-data deploy
chmod -R 775 /var/www
Of course, you might need to change www-data and /var/www to match your setup.





Generate an SSH Key on AWS EC2 Instance:

Step 1: Generate an SSH Key
Note: If you already have an ssh key then you can skip this step
$ ssh-keygen -t rsa
After you hit enter, you’ll get some messages similar to these:

Enter file in which to save the key (/you/.ssh/id_rsa):<br />
Enter passphrase (empty for no passphrase):<br />
Enter same passphrase again:<br />
Your identification has been saved in /you/.ssh/id_rsa.<br />
Your public key has been saved in /you/.ssh/id_rsa.pub.

Step 2: Add Your Key to Your Amazon EC2 Instance
Use the following command to copy your key to your Amazon EC2 instance. /you/.ssh/id_rsa.pub is the location to your ssh key, pem_file.pem is the .pem file you normally use to login, and user@ec2-instance.com is the user and hostname to your EC2 instance:
$ cat /you/.ssh/id_rsa.pub | ssh -i pem_file.pem user@ec2-instance.com "cat >> .ssh/authorized_keys"









Change or Add SSH keys on AWS EC2:
1. Generate a New Private Key
1.	Login to the AWS EC2 console and select Key Pairs in the left sidebar
2.	On the next page, click the Create Key Pair button
3.	Give the new key a name, then click the create button
4.	Download the new key, and then chmod it to 0666
For this tutorial we’ll call this new private key NewKey.pem
2. Generate a New Public Key
Next, use NewKey.pem that was created in step 1 to create a new public key. The new public key will be NewKey.pub.
ssh-keygen -y
When prompted, enter the path to the NewKey.pem that was created in step 1.
3.Add the new public key to your instance

cat NewKey.pub | ssh -i OriginalKey.pem user@amazon-instance "cat >> .ssh/authorized_keys"

4. Test the New Key
Use the new key to ssh into the server. If needed, the original key can be removed during this step
ssh -i NewKey.pem user@amazon-instance 	
To remove the original key use:

nano ~/.ssh/authorized_keys 
Find the line containing the original/old key and remove it.


