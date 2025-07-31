
![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-126.png?w=1011)

Navigate over to **S3** then select **Create a bucket** and name it whatever you would like. Scroll down and click **Create bucket**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-127.png?w=1013)

I am now uploading my application code to the S3 bucket

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-128.png?w=1024)

We can see the file has been upload successfully to our bucket

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-3.png?w=811)

I am creating a VPC and giving it the name of Dev VPC with a CIDR block of 10.0.0.0/16

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-4.png?w=1009)

Next I am enabling DNS hostnames

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-6.png?w=1024)

Now we are going to add an intent gateway to our VPC. When on our VPC click on **Internet gateways** in the side bar to the left. You can only have 1 Internet gateway for 1 VPC. To make sure you add the Internet gateway to the correct VPC click on the box labeled **Filter by VPC** and select the one named **Dev VPC** and finally click on **Create Internet gateway**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-7.png?w=1011)

From there we are going to name our internet gateway to **Dev Internet Gateway** and click on **Create internet gateway**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-8.png?w=197)

Now we need to attach this Internet gateway to our VPC, so click on **Actions** and **Attach to VPC**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-9.png?w=1011)

From there click on the box labeled **Select a VPC**. This will only display VPC that do not have an internet gateway attached. Click on our VPC named **Dev VPC** and click **Attach internet gateway**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-10.png?w=1024)

We need are going to create some subnets. Click on **Subnets** in the menu on the left. Then click on **Create subnet**.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-11.png?w=813)

We are going to select the **Dev VPC**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-12.png?w=811)

For the **Public Subnet** we will do the following;

```
Name - Public Subnet AZ1

Availability Zone - us-east-1a

IPv4 subnet CIDR block - 10.0.0.0/24
```

For the **Private App Subnet** we will do the following;

```
Name - Private App Subnet AZ1

Availability Zone - us-east-1a

IPv4 subnet CIDR block - 10.0.2.0/24
```

For the **Private Data Subnet** we will do the following;

```
Name - Private Data Subnet AZ1

Availability Zone - us-east-1a

IPv4 subnet CIDR block - 10.0.4.0/24
```

Now that we have the 3 subnets created we want to make them highly available. To do this we will created 3 more subnets and add them to a different availability zone.

For the **Public Subnet** we will do the following;

```
Name - Public Subnet AZ2

Availability Zone - us-east-1b

IPv4 subnet CIDR block - 10.0.1.0/24
```

For the **Private App Subnet** we will do the following;

```
Name - Private App Subnet AZ2

Availability Zone - us-east-1b

IPv4 subnet CIDR block - 10.0.3.0/24
```

For the **Private Data Subnet** we will do the following;

```
Name - Private Data Subnet AZ2

Availability Zone - us-east-1b

IPv4 subnet CIDR block - 10.0.5.0/24
```

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-15.png?w=1024)

Next we are going to enable Auto-assign IP settings for the public subnets. This way any app that we have on the public subnet will automatically assigned a IPv4 address.

Next we are going to enable Auto-assign IP settings for the public subnets. This way any app that we have on the public subnet will automatically assigned a IPv4 address.

Select **Public Subnet AZ1** then click on **Actions** and select **Edit subnet settings**.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-16.png?w=1007)

Under Auto-assign IP settings click **Enable auto-assign public IPv4 address**. Scroll down and click on **Save**. Repeat this same process for the other public subnet **Public Subnet AZ2**.

Next we need to make our Public Subnets public. Just naming them public does not make it so they’re public. To do this we need to create a routing table.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-17.png?w=1024)

Under the VPC dashboard click on **Route tables** then select **Create route table**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-18.png?w=1011)

We are going to name it **Public Route Table**. Then select the VPC **Dev VPC** and click on the **Create route table** button.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-19.png?w=1024)

You can see we already have a route created by default and this is a local route, but we need to make sure we create a route that can get to the internet. Click on the **Edit routes** button.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-20.png?w=1024)

Click on **Add route** in networking if we want something to get onto the internet the destination will just be **0.0.0.0/0**. For the target select **Internet Gateway**, then click the box with **igw-** and select **Dev Internet Gateway**. Then click on the **Save Changes** button.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-21.png?w=1024)

We have created a new route. Now any subnet that we associate with this route table will be public.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-22.png?w=1024)

To associate the subnets with this route we click on **Subnet associations** then click on **Edit subnet associations** and select the two public subnets we created.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-23.png?w=1024)

We do not have to do anything to make the private subnets private. If you look at the route tables we will see we have two of them. The one we just made and the one that is made by default when creating a VPC. The default one is the **main** route table.

You can see on our Public Route Table we created we have 2 subnets that are **Explicit subnet associations**. What this means is that we explicitly associated those two subnets to that route table. However, since we have not explicitly associated our other subnets that will be under the main route table by default. And the main route table by default is private.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-24.png?w=1024)

We can verify this buy selecting the route and clicking on **route** as we can see it only has a local route and no routes that are going to the internet.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-25.png?w=1024)

Under **Subnet associations** we can see that our other private subnets are not under explicit association but they are under **Subnets without explicit associations**. AWS explains that this means “_The following subnets have not been explicitly associated with any route tables and are therefore associated with the main route table_“

Next we are going to setup the NAT Gateway. The NAT Gateway allows for the resources on a subnet to have access to the internet without allowing the internet access to the resources in your private subnet. _Which is useful for example an EC2 instance connecting to the internet to download an update._

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-26.png?w=1024)

In the VPC Dashboard select **NAT Gateway** and click on **Create NAT gateway**.

One thing to always remember when creating a NAT Gateway is that they always go in the Public Subnet. So I am going to select **Public Subnet AZ1** but you could Select **Public Subnet AZ2** or use both if you are going to create two NAT Gateways for HA and Fault Tolerance.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-27.png?w=1012)

We are going to do the following options;

NAT Gateway – **Nat Gateway AZ1**

Subnet – **Public Subnet AZ1**

Connectivity type – **Public**

Elastic IP allocation ID – **Click on Allocate Elastic IP**

(We are clicking on that because we do not have any Elastic IP allocations)

Then click the **Create NAT Gateway** button

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-28.png?w=1024)

Head back to the Route tables section in the VPC Dashboard and click **Create route table**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-29.png?w=1011)

Name it **Private Route Table** and select our VPC **Dev VPC** and click on **Create route table**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-30.png?w=1024)

Under the **Routes** tab click on **Edit routes**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-31.png?w=1024)

We will click on **Add route** and add the following options;

Destination – 0.0.0.0/0

Target – NAT Gateway

nat- **Nat Gateway AZ1**

Click on **Save Changes**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-32.png?w=1024)

Click on **Subnet associations** and click **Edit subnet associations**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-33.png?w=1024)

Select all the private subnets and click **Save**

Next we are going to be creating 5 Security Groups

```
Application Load Balancer

It will allow inbound traffic on port 80 and 443 from anywhere on the internet
```

```
EICE Security Group (For our EC2 Connect Endpoint)

It will only allow port 22 on the outbound traffic, source is limited to VPC CIDR block
```

```
App Server Security Group

It will allow inbound traffic on port 80 and 443 from only App Load Balancer Security Group

It will allow inbound traffic on port 22 only from EICE Security Group
```

```
Data Migrate Server Security Group

It will allow inbound traffic on ssh port 22 only from EICE Security Group
```

```
Database Security Group

It will allow inbound traffic mysql port 3306 from the App Server Security Group & Data Migrate Server Security Group
```

We will be using the EC2 Instance Connect Endpoint for SSH that way we do not have to manage SSH keys

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-42.png?w=1024)

From the VPC Dashboard click **Endpoints** then **Create endpoints**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-44.png?w=1024)

For name we will use **Dev EICE** then select **EC2 instance Connect**

For VPC select **Dev VPC**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-46.png?w=1024)

For Security Groups select **EICE Security Group**

Under Subnet select any Private Subnet, I am going to select **Private App Subnet**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-129.png?w=1013)

Navigate over to **RDS** -> **Subnet groups** then select **Create DB subnet group**. Give it a name then select your VPC

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-130.png?w=1008)

I am selecting the two AZs I have subnets in and then picking the two **Private Data Subnets**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-112.png?w=1024)

Navigate to **Databases** under **RDS** and select **Create database**

then select these following options;

```
1. Standard create
2. MySQL
3. Engine Version - Latest Version
4. Single DB instance
5. DB instance identifier - dev-rds-db
6. Master username - Enter a username
7. Self managed
8. Master password - Enter a password
```

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-114.png?w=1003)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-115.png?w=1007)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-116.png?w=1011)

Scroll down and click on **Create database**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-131.png?w=1013)

I am going to create a new **S3** buck for sql files.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-132.png?w=1009)

I am not uploading a sql script to the bucket

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-133.png?w=1024)

I am going to create a new IAM role for this project. Use case will be **EC2**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-134.png?w=1024)

The permissions will be **AmazonS3FullAccess**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-135.png?w=1024)

I will name it **S3role**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-136.png?w=986)

I will being keeping majority of the settings as default, as well as using the Amazon Linux AMI

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-137.png?w=987)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-138.png?w=982)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-139.png?w=981)

```
S3_URI=s3://km-sql-files/V1__shopwise.sql
RDS_ENDPOINT=dev-rds-db.cpcw0aw4astu.us-east-1.rds.amazonaws.com
RDS_DB_NAME=applicationdb
RDS_DB_USERNAME=admin
RDS_DB_PASSWORD=password

# Update all packages
sudo yum update -y

# Download and extract Flyway
sudo wget -qO- https://download.red-gate.com/maven/release/com/redgate/flyway/flyway-commandline/10.9.1/flyway-commandline-10.9.1-linux-x64.tar.gz | tar -xvz 

# Create a symbolic link to make Flyway accessible globally
sudo ln -s $(pwd)/flyway-10.9.1/flyway /usr/local/bin

# Create the SQL directory for migrations
sudo mkdir sql

# Download the migration SQL script from AWS S3
sudo aws s3 cp "$S3_URI" sql/

# Run Flyway migration
flyway -url=jdbc:mysql://"$RDS_ENDPOINT":3306/"$RDS_DB_NAME" \
  -user="$RDS_DB_USERNAME" \
  -password="$RDS_DB_PASSWORD" \
  -locations=filesystem:sql \
  migrate
```

I am going to run these commands to install flyway onto the instance to migrate the sql script from the S3 bucket to the EC2 instance. Then migrate it from the sql folder on the EC2 instance to the RDS database

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-140.png?w=1024)

Now I can go ahead and terminate the data-migrate-instance since I no longer will be needing it

Under EC2 click on **Target groups** and select **Create target group**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-57.png?w=701)

Scroll down and click **Next**, then scroll down again and click **Create target group**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-141.png?w=1014)

When we set up our domain name in Route53 we will be redirecting it to HTTPS and these are the success codes for HTTPS

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-142.png?w=979)

I am going to launch a new EC2 instance, with the **Amazon Linux AMI**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-143.png?w=983)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-144.png?w=979)

Under the EC2 menu go to **Target groups** and scroll down to **Registered targets** and select **Register targets**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-62.png?w=1024)

Verify it is under the **Review targets** tab and click on **Register pending targets**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-63.png?w=1024)

Next we are going to create the application load balancer. Under the EC2 tab scroll down and select **Load balancer** then click **Create load balancer**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-64.png?w=701)

Scroll down and click **Create** under Application Load Balancer

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-65.png?w=1024)

For the name give it **Dev-ALB**, the rest should be as shown

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-66.png?w=1024)

Select the **Dev VPC**

For Availability Zones under Mappings we will check both of the AZs that we have subnets in. For an Application Load Balancer it will only work on a Public Subnet so lets select **Public Subnet AZ1** and **Public Subnet AZ2**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-67.png?w=1024)

For the Security groups click the **X** on the default security group that is selected and then click the drop down and select **ALB Security Group**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-68.png?w=1024)

For Listeners and routing select **Dev-TG** in Default action.

For now we are only going to create an HTTP listener. When we register an SSL Certificate we will create an HTTPS Listener to redirect our HTTP traffic to HTTPS

Scroll down and click **Create load balancer**

### This command indicates that the script should be interpreted and executed using the Bash shell  
#!/bin/bash

### This command updates all the packages on the server to their latest versions  
sudo yum update -y

### This series of commands installs the Apache web server, enables it to start on boot, and then starts the server immediately  
sudo yum install -y httpd  
sudo systemctl enable httpd  
sudo systemctl start httpd

### This command installs PHP along with several necessary extensions for the application to run  

```
sudo dnf install -y \  
php \  
php-pdo \  
php-openssl \  
php-mbstring \  
php-exif \  
php-fileinfo \  
php-xml \  
php-ctype \  
php-json \  
php-tokenizer \  
php-curl \  
php-cli \  
php-fpm \  
php-mysqlnd \  
php-bcmath \  
php-gd \  
php-cgi \  
php-gettext \  
php-intl \  
php-zip
```
### These commands Installs MySQL version 8  

### Install the MySQL Community repository  
```
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm
```
  

### Install the MySQL server  
```
sudo dnf install -y mysql80-community-release-el9-1.noarch.rpm  
sudo rpm –import [https://repo.mysql.com/RPM-GPG-KEY-mysql-2023](https://repo.mysql.com/RPM-GPG-KEY-mysql-2023)  
dnf repolist enabled | grep “mysql.*-community.*”  
sudo dnf install -y mysql-community-server  
```

### Start and enable the MySQL server  
```
sudo systemctl start mysqld  
sudo systemctl enable mysqld
```

### This command enables the ‘mod_rewrite’ module in Apache on an EC2 Linux instance. It allows the use of .htaccess files for URL rewriting and other directives in the ‘/var/www/html’ directory
```
sudo sed -i ‘//,// s/AllowOverride None/AllowOverride All/’ /etc/httpd/conf/httpd.conf
```

### Environment Veriable 
```
S3_BUCKET_NAME=km-project-web-files
```

### This command downloads the contents of the specified S3 bucket to the ‘/var/www/html’ directory on the EC2 instance
```
sudo aws s3 sync s3://”$S3_BUCKET_NAME” /var/www/html
```

### This command changes the current working directory to ‘/var/www/html’, which is the standard directory for hosting web pages on a Unix-based server  
```
cd /var/www/html
```

### This command is used to extract the contents of the application code zip file that was previously downloaded from the S3 bucket  
```
sudo unzip shopwise.zip
```

### This command recursively copies all files, including hidden ones, from the ‘shopwise’ directory to the ‘/var/www/html/’
```
sudo cp -R shopwise/. /var/www/html/
```

### This command permanently deletes the ‘shopwise’ directory and the ‘shopwise.zip’ file.
```
sudo rm -rf shopwise shopwise.zip
```

### This command set permissions 777 for the ‘/var/www/html’ directory and the ‘storage/’ directory 
```
sudo chmod -R 777 /var/www/html  
sudo chmod -R 777 storage/
```

### This command will open th vi editor and allow you to edit the .env file to add your database credentials
```
sudo vi .env

Next I am going to connect to the instance and install the web server

APP_NAME=”Your App” 
```
### Disable debug mode by changing APP_DEBUG=false when your site is ready for live.
```
APP_DEBUG=true 
APP_ENV=production
```
### Must change APP_URL to your website URL. Example: APP_URL=http://your-domain.com 
```
APP_URL=http://Dev-ALB-1584144756.us-east-1.elb.amazonaws.com  
APP_KEY=base64:SbzM2tzPsCSlpTEdyaju8l9w2C5vmtd4fNAduiLEqng=  
LOG_CHANNEL=daily

BROADCAST_DRIVER=log  
CACHE_DRIVER=file  
QUEUE_CONNECTION=sync  
SESSION_DRIVER=file  
SESSION_LIFETIME=120

REDIS_CLIENT=predis  
REDIS_HOST=127.0.0.1  
REDIS_PASSWORD=null  
REDIS_PORT=6379
```

### Change to your database credentials
```
DB_CONNECTION=mysql 
```
### If you use Laravel Sail, just change DB_HOST to DB_HOST=mysql  
### On some hosting DB_HOST can be localhost instead of 127.0.0.1  
DB_HOST=dev-rds-db.cpcw0aw4astu.us-east-1.rds.amazonaws.com  
DB_PORT=3306  
DB_DATABASE=applicationdb  
DB_USERNAME=admin  
DB_PASSWORD=password  
DB_STRICT=false

### You should change this value to hide your admin URL (DON’T use special characters). Example: ADMIN_DIR=hello-admin then your admin URL will be  
# [http://your-domain.com/hello-admin](http://your-domain.com/hello-admin)  
ADMIN_DIR=admin

CMS_ENABLE_INSTALLER=true

I changed the settings I needed to so that it will connect to my RDS DB

### This command will restart the Apache server  
sudo service httpd restart

Then I will run this command to restart the web server

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-145.png?w=1024)

By doing to the application load balancer DNS url we can see the site is up and running

We are going to head over to Route 53 and register a domain. I will not be showing that since it involves filling out sensitive data.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-73.png?w=1024)

Next we are going to create an A record, so navigate over to **Route 53** select on **Hosted zones** and click on your domain.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-74.png?w=929)

From there you will click on **Create record**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-75.png?w=1024)

We will use the following information

Then scroll down and click **Create record**

Let’s up date the URL to our domain

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-76.png?w=1024)

Navigate to **Certificate Manager** by using the search bar and click on **Request a certificate**

Select **Request a public certificate** then click on **Next**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-77.png?w=1004)

Enter your domain name then select **Add another name to this certificate**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-78.png?w=1010)

Now we are going to add a wildcard for our domain so enter ***.** followed by your domain name

Scroll down and click on **Create**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-79.png?w=1024)

Under Domains click on **Create in Route 53** then select the two domains we have created

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-80.png?w=1024)

Navigate back to EC2 then load balancers and select your load balancer

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-81.png?w=1024)

Under the tab Listeners and rules click **Add listener**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-82.png?w=806)

Set the protocol to **HTTPS** then under Routing actions select **Forward to target groups** and select your target group from the drop down

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-83.png?w=1006)

For Certificate source select **From ACM** since we registered the certificate with Amazon Certification Manager

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-84.png?w=811)

Head back to the load balancer and edit the listening for HTTP port 80

We will change the Routing actions to **Redirect to URL** then select **Full URL**

Scroll down and click **Save**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-146.png?w=1024)

```
cd /var/www/html
la -a
sudo vi .env
```

I am going to update this part of the value for app url

```
APP_URL=https://awstower.com
```

```
cd apps
cd Providers
sudo vi AppServiceProvider.php
```

Then we are going to edit this area to as follows;

```
public function boot(): void
    {
            if (env('APP_ENV') === 'production') {\Illuminate\Support\Facades\URL::forceScheme('https');}
        //
    }
```

We want out Auto Scaling group to dynamically create our EC2 instance so first head to the EC2 instance we created and change the instance state to **Terminate**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-86.png?w=1024)

From the EC2 menu select **Launch template** and click on **Create launch template**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-87.png?w=978)

Name it whatever you want **Dev-Launch-Template** and check the box for **Auto Scaling guidance**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-88.png?w=979)

Select **Quick Start** and click **Amazon Linux**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-89.png?w=980)

Select the **t2.micro**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-90.png?w=981)

For the security group select the **App Sever Security Group** we created

Scroll down and click the arrow for **Advanced details** then scroll down to **User data – _optional_**

so it should look like this

```
# This command indicates that the script should be interpreted and executed using the Bash shell
#!/bin/bash

# This command updates all the packages on the server to their latest versions
sudo yum update -y

# This series of commands installs the Apache web server, enables it to start on boot, and then starts the server immediately
sudo yum install -y httpd
sudo systemctl enable httpd 
sudo systemctl start httpd

# This command installs PHP along with several necessary extensions for the application to run
sudo dnf install -y \
php \
php-pdo \
php-openssl \
php-mbstring \
php-exif \
php-fileinfo \
php-xml \
php-ctype \
php-json \
php-tokenizer \
php-curl \
php-cli \
php-fpm \
php-mysqlnd \
php-bcmath \
php-gd \
php-cgi \
php-gettext \
php-intl \
php-zip

## These commands Installs MySQL version 8
# Install the MySQL Community repository
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm 
#
# Install the MySQL server
sudo dnf install -y mysql80-community-release-el9-1.noarch.rpm
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
dnf repolist enabled | grep "mysql.*-community.*"
sudo dnf install -y mysql-community-server 
#
# Start and enable the MySQL server
sudo systemctl start mysqld
sudo systemctl enable mysqld

# This command enables the 'mod_rewrite' module in Apache on an EC2 Linux instance. It allows the use of .htaccess files for URL rewriting and other directives in the '/var/www/html' directory
sudo sed -i '/<Directory "\/var\/www\/html">/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/httpd/conf/httpd.conf

# Environment Veriable
S3_BUCKET_NAME=km-project-web-files

# This command downloads the contents of the specified S3 bucket to the '/var/www/html' directory on the EC2 instance
sudo aws s3 sync s3://"$S3_BUCKET_NAME" /var/www/html

# This command changes the current working directory to '/var/www/html', which is the standard directory for hosting web pages on a Unix-based server
cd /var/www/html

# This command is used to extract the contents of the application code zip file that was previously downloaded from the S3 bucket
sudo unzip shopwise.zip

# This command recursively copies all files, including hidden ones, from the 'shopwise' directory to the '/var/www/html/'
sudo cp -R shopwise/. /var/www/html/

# This command permanently deletes the 'shopwise' directory and the 'shopwise.zip' file.
sudo rm -rf shopwise shopwise.zip

# This command set permissions 777 for the '/var/www/html' directory and the 'storage/' directory
sudo chmod -R 777 /var/www/html
sudo chmod -R 777 storage/

# This command will open th vi editor and allow you to edit the .env file to add your database credentials 
sudo vi .env

# This command will restart the Apache server
sudo service httpd restart
```

Then click **Create launch template**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-93.png?w=1024)

Under the EC2 instance select **Auto Scaling Group** and click **Create Auto Scaling group**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-94.png?w=1010)

For the name I will use **Dev-ASG** and for Launch template I will select the one we just created. Scroll down and click **Next**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-95.png?w=1008)

Scroll down to **Network** and select your VPC **DEV VPC** and for the availability Zones select the two **Private App Subnets AZ1** and **AZ2** that we created click on **Next**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-96.png?w=914)

Select **Attach to an existing load balancer** then select our Application Load Balancer we created

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-97.png?w=911)

Check the box for **Turn on Elastic Load Balancing health checks** scroll down and click **Next**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-98.png?w=915)

For Desired capacity changed it to **2**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-99.png?w=912)

For Scaling enter **1** for the Min and **3** for the Max.

_What this means is that minimum capacity we will scale to is one EC2 instance and the maximum capacity we will scale to will be three EC2 instances._

Keep the **Instance maintenance policy** set to **No Policy** scroll down and click **Next**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-100.png?w=911)

If you would like to add a notification for the EC2 auto scaling group click **Add notification** then select **Create topic**. Give it a name and enter the email address you want to get the notifications. Scroll down then click on **Next**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-101.png?w=912)

Lets add a tag for this ASG. For the key enter **Name** and for the value enter **Dev-ASG-Server**. Scroll down and click **Next** then at the very bottom select **Create Auto Scaling Group**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-103.png?w=1024)

We can now see the two EC2 instances that we created with the ASG.