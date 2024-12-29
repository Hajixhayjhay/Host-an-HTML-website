Hosting an HTML Website on EC2

This project demonstrates the process of hosting a static HTML website on an EC2 instance using two different methods for retrieving web files: one from an S3 bucket and another from a GitHub repository. The project includes the creation of two separate scripts that automate the setup of the EC2 instance, install necessary software, download the website files, and configure Apache HTTPD to serve the website.

## Overview

The goal of this project was to automate the hosting of an HTML website on an EC2 instance by leveraging AWS services such as S3 and GitHub. This was achieved by creating user data scripts that automatically configure the server upon instance launch. The web files were fetched either from an S3 bucket or a public GitHub repository, and the Apache web server was set up to serve these files.

## Project Summary

### Task 1: Hosting HTML Website from S3 Bucket

In Task 1, we downloaded HTML files from an S3 bucket and hosted the website on an EC2 instance. The steps involved were as follows:

1. *Web File Download from S3*  
   We started by downloading the HTML web files (in a ZIP format) from an S3 bucket hosted on AWS. The web files were made available for download via the following link:
   - [Download mole.zip from S3](https://aosnote-assignments.s3.amazonaws.com/assignment-1/mole.zip)

2. *Uploading Files to S3*  
   The mole.zip file was uploaded to an S3 bucket in our AWS account. This made the file accessible for download by the EC2 instance during its launch.

3. *Script for EC2 Setup*  
   A bash script was created to automate the setup process on the EC2 instance. The script performed the following tasks:
   - Installed the Apache HTTPD server.
   - Downloaded the mole.zip file from the S3 bucket.
   - Extracted the contents of the ZIP file to the Apache document root (/var/www/html).
   - Cleaned up the downloaded files and started the Apache service to serve the website.

   The script used for this task is as follows:
  
#!/bin/bash        
sudo su
yum update -y
yum install -y httpd
cd /var/www/html
aws s3 cp s3://ajbuckets/mole.zip /var/www/html
unzip mole.zip
cp -r mole/* /var/www/html/
rm -rf mole.zip mole
systemctl enable httpd
systemctl start httpd

4. *User Data Configuration*  
   The script was added to the *User Data* section during the EC2 instance launch. This ensured that the website was automatically set up and ready to be accessed once the EC2 instance was running.

### Task 2: Hosting HTML Website from GitHub Repository

In Task 2, the goal was to download the HTML files from a public GitHub repository and host the website on the same EC2 instance. The steps involved were as follows:

1. *Web File Download from GitHub*  
   The mole.zip file was uploaded to a public GitHub repository, making it accessible for download during the EC2 instance setup. The web files were hosted on the following repository:
   - [GitHub Repository for mole.zip](https://github.com/azeezsalu/jupiter)

2. *Script for EC2 Setup*  
   A separate bash script was created for Task 2, with similar functionality but downloading the files from GitHub instead of S3.
   The script used for this task is as follows:
   #!/bin/bash
   sudo su
   yum update -y
   yum install -y httpd
   cd /var/www/html
   yum install git -y
   git clone https://github.com/Hajixhayjhay/Host-an-HTML-website.git 
   cp -r Host-an-HTML-website/* /var/www/html/
   rm -rf Host-an-HTML-website
   systemctl enable httpd
   systemctl start httpd
   

4. *User Data Configuration*  
   This script was also added to the *User Data* section during EC2 instance launch. As a result, the EC2 instance automatically fetched the web files from GitHub and started serving them using Apache HTTPD.

## Acceptance Criteria

- Two separate scripts were created and successfully added to the EC2 instance’s *User Data*.
  1. The first script downloads and sets up the HTML website from an S3 bucket.
  2. The second script downloads and sets up the HTML website from a GitHub repository.
  
- Each script:
  - Installs Apache HTTPD on the EC2 instance.
  - Downloads and extracts the HTML files from either the S3 bucket or GitHub repository.
  - Copies the files to the correct Apache directory (/var/www/html).
  - Starts the Apache HTTPD service and ensures the website is live and accessible.

## Conclusion

This project successfully demonstrated how to host an HTML website on an EC2 instance using automation with bash scripts. By utilizing AWS services such as S3 and GitHub, we were able to automate the process of downloading web files and configuring the server. The scripts created for this project provide a reproducible and scalable solution for hosting static websites in the cloud.
