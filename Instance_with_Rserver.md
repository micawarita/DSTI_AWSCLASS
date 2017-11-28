# How to launch instance with R server

This document is create with reference to the instruction below
https://aws.amazon.com/blogs/big-data/running-r-on-aws/

### 1.	Login to AWS console
-	Login to RosettaHub
-	Click “Go to AWS Console”

### 2.	Click EC2 and Launch Instance
- Choose Amazon Linux AMI 2017.09.1 (HVM), SSD Volume Type
- Choose t2.micro (Free tier eligible) and click “Next: Configure Instance Details”

### 3.	In Configure Instance Details page
-	Go to “advanced details” tab, copy this code to install install R, RStudio Server.

```
#!/bin/bash
#install R
yum install -y R

#install RStudio-Server 1.0.153 (2017-07-20)
#Go to Rsudio page to check and replace URL for the latest and CentOS suitable version
#https://www.rstudio.com/products/rstudio/download-server/
wget https://download2.rstudio.org/rstudio-server-rhel-1.1.383-x86_64.rpm
yum install -y --nogpgcheck rstudio-server-rhel-1.1.383-x86_64.rpm
rm rstudio-server-rhel-1.0.153-x86_64.rpm

#I excluded this code because we don’t need to install shinu-server.
#install shiny and shiny-server (2017-08-25)
R -e "install.packages('shiny', repos='http://cran.rstudio.com/')"
wget https://download3.rstudio.org/centos5.9/x86_64/shiny-server-1.5.4.869-rh5-x86_64.rpm
yum install -y --nogpgcheck shiny-server-1.5.4.869-rh5-x86_64.rpm
rm shiny-server-1.5.4.869-rh5-x86_64.rpm

#add user(s)
useradd mwarita1
echo mwarita1:pwformwarita | chpasswd 
```
### 4.	Stay in Configure Instance Details page and 
-	Change “IAM Role” from default to “EMR_EC2_DefaultRole”
-	Leave other settings as default


### 5.	Leave Configure Instance Details page and 
- Click “Next:Add strage”  and keep the default setting.
- Click “Next:Add Tags” and add nothing.
- Click”Next: Security Group”

### 6.	In “Configuring the security group” page
- Check “create new security group” and set them like below.
    SSH - TCP - 22(default value) - Anywhere - 0.0.0.0/0
    Custom TCP - TCP - 8787 - Anywhere - 0.0.0.0/0
 
It seems
-Choosing type “SSH” for the default proposed Port Range 22
-Adding new type “Custom TCP” and putting 8787 for Port Range is critical matter. 

(I don’t know the reaon. Here are the description from the instruction URL)
In the EC2 launch wizard, you define a security group, which acts as a virtual firewall that controls the traffic for one or more instances. For your R-based analysis environment, you have to open up port 8787 for RStudio Server and port 3838 for Shiny Server.

### 7.	Click “Review and Launch” and “Launch”
- Choose “Choose a key pair” which I created before.

### 8. Check whether I can access R-studio from web-browser
Address will change everytime we launch the instance.
http://ec2-52-16-199-204.eu-west-1.compute.amazonaws.com:8787
 
## Issue
By this procedure, unfortunately, something was wrong with my username & password settings.
Need to figure out why...



