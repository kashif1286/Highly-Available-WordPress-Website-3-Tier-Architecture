
# Highly Available WordPress Website: 3-Tier Architecture

![Blank diagram](https://github.com/user-attachments/assets/078e108b-3a01-44d0-ac0c-7c0ae3fa4fcb)


Description: In this project, you will develop a WordPress website using AWS services, designed for high availability, scalability, and reliability. This setup will use EC2 instances for the application layer, RDS for the database layer

Tools/Technologies:

AWS EC2: For hosting the WordPress application.

Amazon RDS: For managing the MySQL database.

Amazon VPC: To set up secure networking and subnets.

Linux: For configuring EC2 instances.


# Step 1: VPC and Subnet Creation for Highly Available WordPress

1. Create the VPC

Login to the AWS Management Console.

Navigate to the VPC Dashboard by searching for "VPC" in the search bar.

Click on Create VPC.

Name: Give your VPC a descriptive name (e.g., WordPress-VPC).

IPv4 CIDR block: Define the IP range for the VPC (e.g., 10.0.0.0/16).

This range allows you to have up to 65,536 IP addresses.

Tenancy: Choose Default unless you need dedicated hardware.
Click Create VPC.

![vpc](https://github.com/user-attachments/assets/b58949f8-0cd7-4d4a-87d9-b9eb866f7f9f)

![vpc2](https://github.com/user-attachments/assets/2b11929b-69fd-41d0-8a79-930b3e48dc10)

![vpc3](https://github.com/user-attachments/assets/0e4f2c0b-c46a-4551-904a-5bf154b93f17)


2. Create Public Subnets

You will create two public subnets in different Availability Zones.

In the VPC dashboard, go to Subnets and click on Create subnet.

Configure the first public subnet:

VPC ID: Select the VPC you created (WordPress-VPC).

Subnet name: Name it something like Public-Subnet-1.

Availability Zone: Choose an AZ (e.g., us-east-1a).

IPv4 CIDR block: Specify a CIDR block for the first public subnet (e.g., 10.0.1.0/24).

Click Create subnet.

Configure the second public subnet:

Repeat the steps above but select a different AZ (e.g., us-east-1b).

Name this subnet Public-Subnet-2.

Specify a different CIDR block (e.g., 10.0.2.0/24).
Click Create subnet.

![vpc4](https://github.com/user-attachments/assets/f63ce9d1-7052-4227-bb09-f097183ad88c)

![vpc5](https://github.com/user-attachments/assets/d5082c8b-3c64-4127-afab-93f865020851)

![vpc6](https://github.com/user-attachments/assets/535783dd-dacd-47cd-97e3-d43e144fdd42)

![vpc7](https://github.com/user-attachments/assets/bc5913dc-412e-471a-9a1c-4bf84e26ab40)

create 3 private subnet

3. Create Private Subnets
Next, create two private subnets for the database (RDS).

Click Create subnet again.

Configure the first private subnet:

VPC ID: Select the same VPC (WordPress-VPC).
Subnet name: Name it Private-Subnet-1.

Availability Zone: Choose the same AZ as the first public subnet (e.g., us-east-1a).

IPv4 CIDR block: Specify a different CIDR block (e.g., 10.0.3.0/24).

Click Create subnet.

Configure the second private subnet:

Repeat the process for a different AZ (e.g., us-east-1b).

Name this subnet Private-Subnet-2.

Specify a different CIDR block (e.g., 10.0.4.0/24).
Click Create subnet.

![vpc8](https://github.com/user-attachments/assets/1a089395-38b0-48a5-912a-adf85514d699)

![vpc9](https://github.com/user-attachments/assets/d86e3de8-6094-4ab3-96cc-0feec4837f7d)

![vpc10](https://github.com/user-attachments/assets/cdcdc7ef-f9f9-4060-adf0-9b54bec1ab5f)

4.  Enable Auto-Assign Public IP for Public Subnets

This step ensures that EC2 instances in the public subnets automatically get public IP addresses.

Go to Subnets in the VPC dashboard.

Select Public-Subnet-1 and Public-Subnet-2.

Under Actions, click Modify auto-assign IP settings.

Check the box to Enable auto-assign public IPv4 address.
Click Save.

![vpc11](https://github.com/user-attachments/assets/a2cf0aba-5d42-4fa7-9f46-8e84dfd45722)

![vpc12](https://github.com/user-attachments/assets/fcdea947-b91d-4a1a-a27e-793cf1719dc4)

![vpc13](https://github.com/user-attachments/assets/93807e90-c2d8-4203-ba7c-222fa64b8996)

![vpc14](https://github.com/user-attachments/assets/997a1e9d-40b1-430f-8a1e-6f9dfaaaa2b6)

![vpc15](https://github.com/user-attachments/assets/290a3a82-7900-473e-bf21-6c68ef8b52fb)

![vpc16](https://github.com/user-attachments/assets/d366666a-bd0b-45ef-a615-9d3048f2b7cc)


5. Create and Attach an Internet Gateway

To allow instances in the public subnet to access the internet, you need to create an Internet Gateway and attach it to the VPC.

In the VPC dashboard, click Internet Gateways.

Click on Create Internet Gateway.

Name tag: Provide a name (e.g., WordPress-IGW).

Click Create internet gateway.

Once the Internet Gateway is created, select it, click Actions, and choose Attach to VPC.

Select the VPC you created (WordPress-VPC) and click Attach internet gateway.

![vpc17](https://github.com/user-attachments/assets/2d640511-9dc6-4300-a12a-0aa7b12d87aa)

![vpc18](https://github.com/user-attachments/assets/ae56580f-e329-4889-8901-a38b46c50401)

![vpc19](https://github.com/user-attachments/assets/1f39d27b-7c47-466c-99dd-3f1eeb8b0ebc)

![vpc20](https://github.com/user-attachments/assets/9fff1dd1-e0c8-4169-84f9-3f6f23d7a60f)

6.  Create Route Table for Public Subnets

Now, configure the routing for public subnets to use the Internet Gateway.

In the VPC dashboard, click on Route Tables.

Click Create route table.

Name: Name it Public-RT.

VPC: Select your VPC (WordPress-VPC).

Click Create route table.

After creating the route table, select it and go to the Routes tab.

Click Edit routes and add the following route:

Destination: 0.0.0.0/0 (this allows traffic to the internet).

Target: Select Internet Gateway and choose the gateway you created (WordPress-IGW).

Click Save changes.

![vpc21](https://github.com/user-attachments/assets/cf76a65f-2817-439b-9c38-8069f96f6f89)

![vpc22](https://github.com/user-attachments/assets/f53e7a1a-2261-4fe0-9de9-50aa94fa0f40)

![vpc23](https://github.com/user-attachments/assets/a6b6a911-ec98-405e-904d-bddd60f268b5)

![vpc24](https://github.com/user-attachments/assets/78ea9bb0-49b0-4f02-8890-6d65f51e5266)

6. Associate Public Subnets with Public Route Table

To enable internet access for the public subnets, associate them with the public route table.

Go to the Route Table dashboard, select the Public-RT, and click on the Subnet associations tab.

Click Edit subnet associations.

Select the public subnets (Public-Subnet-1 and Public-Subnet-2).

Click Save.

![vpc25](https://github.com/user-attachments/assets/29c4949e-cd05-474e-a201-e64a6d0d5f7f)

![vpc26](https://github.com/user-attachments/assets/6bd2d23c-359b-41ed-ab84-663a75444d45)

8. Create Route Table for Private Subnets

Private subnets should not have direct internet access. Create a route table for them.

Click Create route table.

Name: Name it Private-RT.

VPC: Select your VPC (WordPress-VPC).

Click Create route table.

Do not add an internet route to this route table. It should only have local VPC traffic routing.

Associate this route table with the private subnets (Private-Subnet-1 and Private-Subnet-2).

![vpc27](https://github.com/user-attachments/assets/f759125f-41e9-4fdb-a435-b7075d707cad)

![vpc28](https://github.com/user-attachments/assets/2da04922-1a44-4e4a-a547-305cc6c64e31)

![vpc29](https://github.com/user-attachments/assets/d45d79cf-431f-4cc5-95b3-e7c6bfd02110)

![vpc 30](https://github.com/user-attachments/assets/35c72746-d101-4dda-9416-c2830be6c84a)

# Step 2: Configure Security Groups

1. Create Security Group for EC2 Instances

The Security Group for the EC2 instances will allow HTTP (port 80), HTTPS (port 443), and SSH (port 22) access. The SSH access should be limited to trusted IP ranges, while HTTP and HTTPS traffic will be open to everyone (0.0.0.0/0).

Navigate to the EC2 Dashboard:

Go to the EC2 Dashboard in the AWS Management Console.

In the left-hand menu, click on Security Groups under Network & Security.

Create the Security Group:

Click on Create Security Group.

Configure the Security Group for EC2:

Security Group Name: Provide a descriptive name (e.g., EC2-WordPress-SG).

Description: Write a description, such as "Security group for WordPress EC2 instances."

VPC: Select the VPC you created earlier (e.g., WordPress-VPC).

Inbound Rules: Add the following rules to allow traffic to your EC2 instances:

SSH Access:

Type: SSH

Protocol: TCP

Port Range: 22

Source: Your trusted IP range (e.g., 0.0.0.0/0).

This limits SSH access to your IP only for security.

HTTP Access:

Type: HTTP

Protocol: TCP

Port Range: 80

Source: 0.0.0.0/0 (Allowing access from anywhere).

HTTPS Access:

Type: HTTPS

Protocol: TCP

Port Range: 443

Source: 0.0.0.0/0 (Allowing access from anywhere).

Outbound Rules:


By default, AWS Security Groups allow all outbound traffic.

You can leave this as-is, ensuring the EC2 instances can communicate with the internet.

Create the Security Group:


Once the rules are defined, click Create Security Group.
Associate the Security Group with EC2 Instances:

When launching your EC2 instances in the public subnets (later), attach this Security Group (EC2-WordPress-SG) to them.

![sg1](https://github.com/user-attachments/assets/77121239-bb69-4831-9466-4809ea4cf8ba)

![sg2](https://github.com/user-attachments/assets/5ddff399-881e-4fbe-8b40-eb6ee52843f9)

![sg3](https://github.com/user-attachments/assets/30a2a92f-3dee-4d27-afd6-d888634e70c6)

![sg4](https://github.com/user-attachments/assets/538ccf9c-bc1e-4316-8609-4e9f62f23f91)

![sg5](https://github.com/user-attachments/assets/ccc6c24a-8be7-4d7f-b4ad-1a9416b997cd)

2. Create Security Group for RDS Instances

This Security Group will allow access only from EC2 instances within the VPC, specifically the private subnets, for database traffic (MySQL/Aurora on port 3306).

Create the Security Group:

Navigate to the Security Groups section in the EC2 Dashboard.

Click on Create Security Group.

Configure the Security Group for RDS:

Security Group Name: Provide a descriptive name (e.g., RDS-MySQL-SG).

Description: "Security group for MySQL RDS allowing access only from EC2 instances."

VPC: Select your VPC (e.g., WordPress-VPC).
Inbound Rules:

MySQL/Aurora Access:

Type: MySQL/Aurora

Protocol: TCP

Port Range: 3306

Source: Select Custom and choose the EC2 Security Group (EC2-WordPress-SG). This will ensure that only instances within this Security Group (EC2 instances) can access the RDS database.

Outbound Rules:

Leave the default All traffic rule to allow RDS instances to initiate outbound connections if necessary.

Create the Security Group:

Click Create Security Group to finish.

![sg6](https://github.com/user-attachments/assets/2d8f7bbc-d6b9-4c8c-863b-aa9af4566047)

![sg9](https://github.com/user-attachments/assets/2ee1c3f3-e462-43bc-ac5f-ccd57d1ae123)

![s10](https://github.com/user-attachments/assets/3533bbb5-7500-449a-886a-f473bbed9666)

# Step 3: Launch EC2 Instances for WordPress

Step-by-Step Instructions: Launching EC2 Instances and Installing WordPress

1. Launch EC2 Instances in Public Subnets

Navigate to the EC2 Dashboard:

Go to the EC2 Dashboard in the AWS Management Console.

2. Launch an EC2 Instance:

Click on Launch Instance.

3. Configure EC2 Instance Settings:

Name and Tags: Name your instance (e.g., WordPress-EC2-1).

4. Choose AMI (Amazon Machine Image):

Select Amazon Linux 2 as the AMI (or another Linux distribution of your choice).

5. Choose Instance Type:

Select t2.micro (or larger depending on traffic requirements) under Instance type. This is free-tier eligible for small-scale testing.

6. Configure Network Settings:

VPC: Select the VPC you created earlier (WordPress-VPC).
Subnet: Choose one of the public subnets (Public-Subnet-1 or Public-Subnet-2).

Auto-assign Public IP: Ensure that Auto-assign Public IP is enabled so the instance can be accessed from the internet.

7. Select Security Group:

Use the existing EC2-WordPress-SG Security Group created earlier.

This Security Group allows HTTP, HTTPS, and SSH traffic.

8. Launch EC2 Instance:

Select a key pair to enable SSH access (you can create a new one if needed). Make sure to download the private key 
.

pem file if you create a new key pair.

Click Launch Instance.

9. Repeat the process for the second instance:

Launch another EC2 instance in the second public subnet (Public-Subnet-2), following the same steps as above, and name it WordPress-EC2-2.

![ec1](https://github.com/user-attachments/assets/d0755155-cc36-413e-ac5d-6426c4b43f39)

![ec2](https://github.com/user-attachments/assets/fbc89606-5e97-4629-a766-5cded58dcaf0)

![ec3](https://github.com/user-attachments/assets/6ad44674-aaa6-40a9-bca1-719cbe8b9b58)

![ec4](https://github.com/user-attachments/assets/7387647d-e92d-4f83-8850-0044ecce3cbb)

![ec5](https://github.com/user-attachments/assets/43bd4cbc-3113-4363-8064-36e12a5ae020)

# 3. 0 Install LAMP

You can choose between LAMP (Linux, Apache, MySQL, PHP)

Option 1: Install LAMP Stack (Apache)

connect previous creted instance

![rn1](https://github.com/user-attachments/assets/c95c7111-c7a0-4bc2-9dc7-5454424b6c03)

![rn2](https://github.com/user-attachments/assets/0d1f521e-8eed-43db-8d9f-0a3e70594d2a)

Option 1: Install LAMP Stack (Apache)

```bash
sudo yum update -y

```

![rn3](https://github.com/user-attachments/assets/501fea27-4d61-4484-86d9-6729f9033bd7)


```bash
sudo yum install httpd -y

```

![rn4](https://github.com/user-attachments/assets/6eb21fc1-e9ac-45ea-ae74-abf3609147a8)

```bash
sudo systemctl start httpd
sudo systemctl enable httpd

```


![rn5](https://github.com/user-attachments/assets/40408935-5ad7-49de-ac73-da28b13d6483)


```bash
sudo yum install php php-mysqlnd -y

```

![rn6](https://github.com/user-attachments/assets/0885d7f3-9e8d-491c-857e-34bd488224ba)

```bash
sudo yum install mariadb-server -y
sudo systemctl start mariadb
sudo systemctl enable mariadb


```


![rn7](https://github.com/user-attachments/assets/99d81af1-a9a6-4fcc-9602-bd9ece7e267d)

you have a problem mariadb-server is not install then you next command try 

![rn8](https://github.com/user-attachments/assets/d059ab3f-9453-4c20-a108-4ad559ba28ae)


```bash
sudo rm /etc/yum.repos.d/MariaDB.repo

```

![rn9](https://github.com/user-attachments/assets/88b0b0b5-138d-4b87-b2ef-12935e728ec4)


```bash
sudo yum install mariadb105-server mariadb105 -y
```

![rn10](https://github.com/user-attachments/assets/394f8e6a-1203-49ee-9906-6c5269a057a8)

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb

```

![rn11](https://github.com/user-attachments/assets/be466d65-99b4-4dff-902d-af33d0f757c1)

Verify PHP Installation:

Create a PHP info file:

```bash
sudo echo "<?php phpinfo(); ?>" > /var/www/html/info.php

```

![rn12](https://github.com/user-attachments/assets/64ac6b52-c9a9-4da5-8028-b4963c663cff)

when you permission denied then you run this command other then you not run 

```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php


```

![rn14](https://github.com/user-attachments/assets/b3c95de1-9c29-4665-ae95-9817c84f918c)

Visit (http://<Public-IP>/info.php) to confirm PHP is working.

copy your current running instnace of public ip and paste here (http://<Public-IP>/info.php)

![rn13](https://github.com/user-attachments/assets/32ac7e5a-4f4d-4a7a-96ab-6db001be26eb)

paste public ip 

![rn15](https://github.com/user-attachments/assets/ee273884-fc35-41f6-a284-3dd296e90024)

your successful step 

![rn16](https://github.com/user-attachments/assets/7525f8c0-c178-43c0-8c6d-959a843a5e83)

Install and Configure WordPress

Download WordPress:

Navigate to the web directory and download WordPress:

```bash
cd /var/www/html
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xzf latest.tar.gz
sudo mv wordpress/* ./


```

![rn17](https://github.com/user-attachments/assets/cabcf144-fc57-4a16-a6ba-637bd2659cf2)

```bash

sudo chown -R apache:apache /var/www/html
sudo chmod -R 755 /var/www/html


```


![rn18](https://github.com/user-attachments/assets/e475f955-1cf3-4457-9383-d232511918c6)

reate WordPress Configuration File:

Copy the sample WordPress config file

```bash
sudo cp wp-config-sample.php wp-config.php

```

![rn19](https://github.com/user-attachments/assets/ec429e1a-d3ce-4903-b249-df79aa4e4a8b)

Configure the WordPress Database:

Create a new database for WordPress in MySQL:

```bash
sudo mysql -u root -p
CREATE DATABASE wordpress;
CREATE USER 'wp_user'@'%' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wp_user'@'%';
FLUSH PRIVILEGES;
EXIT;


```

then you enter your password (12345678)
are you enter in mariadb server successfully 
next exit 

![rds-problem5](https://github.com/user-attachments/assets/6b775599-a005-4d02-9d26-b3b9ad9306cc)

#  Step 4: Set Up RDS for WordPress Database

In this step, you will create an RDS instance with Multi-AZ deployment for redundancy and high availability, using either MySQL or Amazon Aurora. The RDS instance will be placed in the private subnet, and you will enable automatic backups. Finally, you'll configure WordPress to connect to the RDS database.

Step-by-Step Instructions: Setting Up RDS and Connecting WordPress

1. Launch RDS Instance (MySQL or Amazon Aurora)
Navigate to the RDS Dashboard:

In the AWS Management Console, go to the RDS Dashboard.

2. Create Database:

Click Create database.

3. Choose Database Creation Method:

Select Standard create.

4. Engine Options:

Choose MySQL or Amazon Aurora based on your preference.

5. Version:

For MySQL, choose the latest stable version.

For Aurora, choose a compatible version of Aurora MySQL.

6. Database Instance Specifications:

DB Instance Class: Select an instance class that matches your expected workload.

 For testing purposes, you can choose db.t3.micro (free tier eligible) or a larger instance if required.

Multi-AZ Deployment: Check the Multi-AZ option for high availability. 

This will create a secondary standby instance in another availability zone.

7. Settings:

DB Instance Identifier: Name your database (e.g., WordPress-RDS).

Master Username: Set the username (e.g., admin).

Master Password: Set a strong password and confirm it.

8. DB Instance Connectivity:

VPC: Choose the VPC created earlier (e.g., WordPress-VPC).

Subnets: Select the private subnets created for the database.

Public Access: Ensure No is selected for public access, as this will keep the database private.

Security Group: Select the RDS-MySQL-SG Security Group that only allows access from the EC2 instances (created earlier).

9. Database Storage:

Choose storage type, and enable Auto-scaling to allow automatic increases in storage capacity when needed.

10. Enable Automatic Backups to protect your data:

Set the Backup retention period (e.g., 7 days).

11. Encryption:

Enable encryption if desired for enhanced security (optional).

12. Database Authentication:

Use password authentication for simplicity, though you can enable IAM authentication if you prefer.

13. Monitoring:

Enable Enhanced Monitoring and set the monitoring level based on your needs.

14. Maintenance:

Enable Auto minor version upgrade to apply minor DB version updates during the maintenance window.

15. Launch the RDS Instance:

Review the settings and click Create database


![rds1](https://github.com/user-attachments/assets/333e6030-2d00-45a2-b7cf-ac7c35f6718c)

![rds2](https://github.com/user-attachments/assets/15ad699f-79d1-4149-8460-084acde30c82)

![rds-problem](https://github.com/user-attachments/assets/56f3dd18-7ef4-4f8d-9419-93fd73277bff)

![rds4](https://github.com/user-attachments/assets/78ae2072-121b-43c3-b2d1-4a70b0540829)

you enter a password 12345678

![rds5](https://github.com/user-attachments/assets/58ba8ba5-44ac-4d45-ae05-cfd2843fd8f1)


![rds6](https://github.com/user-attachments/assets/e74b0d74-5927-42fd-a160-adf32605f1cb)

open a new tab and go RDS service an create a subnet group for availability 

enter name.

enter DISCRIPTION .

chose a pervious creted vpc .

slsect 3 diffrent availability zone.

![rds-sg1](https://github.com/user-attachments/assets/f50f4788-54d0-4743-bd54-945bf785a13e)

and choose 3 private subnet . 

and create a subnet group

![rds-sg2](https://github.com/user-attachments/assets/b816693d-0547-4efa-ae5b-1c28e032444a)

go to return  current RDS service and start process .

![rds-sg3](https://github.com/user-attachments/assets/ac6cf170-f1ff-4c18-ae87-68efd6bff72f)

select only RDS-security group

![rds-sg4](https://github.com/user-attachments/assets/c39e206d-6b88-4ba4-ac61-54425fd87587)

create database 

![rds-sg5](https://github.com/user-attachments/assets/539205fe-cfc3-4618-967d-dba4bc51c9ce)

hold on 5 min and cheak you RDS instance active status

![rds-problem2](https://github.com/user-attachments/assets/93fb7f3c-52fc-4990-85d9-f2f6b0c97ced)

then go to terminal 

Edit the wp-config.php File:

Open the wp-config.php file located in the /var/www/html directory

```bash
sudo vi /var/www/html/wp-config.php


```


![rdrun1](https://github.com/user-attachments/assets/485494b4-09f7-4b14-811b-cb8a6cac681d)


but 1stly you copy your RDS database of endpoint 

![rds-problem3](https://github.com/user-attachments/assets/5282f2f2-338c-4ebb-9de4-2fea65810c2d)

Update the Database Connection Details:

Find the following lines in the wp-config.php file

```bash
define('DB_NAME', 'database_name_here');
define('DB_USER', 'username_here');
define('DB_PASSWORD', 'password_here');
define('DB_HOST', 'localhost');

```
Replace these lines with the following, using the RDS endpoint and database details

```bash
define('DB_NAME', 'wordpress');
define('DB_USER', 'admin');
define('DB_PASSWORD', 'your_password');
define('DB_HOST', 'your_rds_endpoint');


```
your password :- 12345678

copy your rds endpoint and paste 


![rdrun2](https://github.com/user-attachments/assets/0001d014-ce40-41a7-8063-3cc615a7e3d5)

 Test the WordPress Setup

```bash
sudo systemctl restart httpd

```
![rdrun3](https://github.com/user-attachments/assets/e4a6de02-9299-40f2-bdf6-ef81cb7f8c12)

Access WordPress:

Visit http://<Public-IP> in your browser and complete the WordPress setup process. The database connection should now be handled by RDS.

copy your instance public ip adress and paste

then your web site is error next you run next command 

![rdrun4](https://github.com/user-attachments/assets/6fac7338-d215-409b-b4fe-8159dc924f08)



```bash
mysql -h wordpress-rds-instance-1.cxc8kccketxl.us-east-1.rds.amazonaws.com -u admin -p

```
copy your upload rds endpoint and paste here 

```bash
mysql -h < RDS Endpoint copy and paste > -u admin -p

```
then enter your password (12345678)

start your maria db

![rds-problem7](https://github.com/user-attachments/assets/ac9ca7d4-b2b6-4d85-b0bb-e54cadce90ff)

Check for Existing Database:

If you can connect, ensure that the (wordpress) database exists on the RDS instance. You can check this by running:

```bash
SHOW DATABASES;

```

![rds-problem8](https://github.com/user-attachments/assets/ecee5ddb-c2db-4cf8-b427-8c3d4362701f)


If the wordpress database does not exist, create it with

```bash
CREATE DATABASE wordpress;

```

![rds-problem9](https://github.com/user-attachments/assets/e61ade10-2804-43d5-a4b3-3203469071bd)

copy your ec2 instnace public ip adress and paste website 

Access WordPress:

Visit ( http://<Public-IP> ) in your browser and complete the WordPress setup process. The database connection should now be handled by RDS.

![cong1](https://github.com/user-attachments/assets/39c85ee2-b0b2-40fc-8126-231ac9692137)

congratulations your project run.

click a continue button 

![cong2](https://github.com/user-attachments/assets/937c8ab7-8c16-4f20-bd30-829b3cbfe3ef)

enter .
 username:- admin

 password:- 12345678

 confirm password

 your email

install wordpress

![cong3](https://github.com/user-attachments/assets/25a665b6-a42e-4a15-a8ad-582ea5f419df)

your Installation successful

login 

![cong4](https://github.com/user-attachments/assets/f4a4d0b4-8cb7-4514-8835-8152fd93a627)

enter your email, and password

login 

![cong5](https://github.com/user-attachments/assets/e8a62aa8-4c82-4132-81a9-6504f694abd1)

wlcome to wordpress 

![cong6](https://github.com/user-attachments/assets/b5d5ddd2-d252-4182-bb8f-1f3b1b819e85)





















