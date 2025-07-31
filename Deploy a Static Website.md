
![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/host_a_static_website_on_aws.gif?w=576)

This is the reference architecture of the project we are going to create

##### Step 1: Creating IAM User

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image.png?w=1024)

We’re going to first create a user named **Cloud-Project-User** and give them **AdministratorAccess**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-1.png?w=739)

Once the user is created click on the user and go to the **Security Credentials** tab. Scroll down until you see **Access Keys** click on **Create access key** select the **Command Line Interface (CLI)** option to allow them access to the AWS account with the CLI. Click on **Next** and then **Create access key**. Now we have an access key for the user.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/cli.png?w=882)

To configure IAM user’s access key on Windows open up **Powershell** and type in the command **aws configure**. It will ask for the user’s **Access Key** and **Secret Access Key**. Just enter information you got when creating the access key. Then it will ask what region name you are using for this I will be using **us-east-1**.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-2.png?w=826)

Now let’s verify that the CLI is working correctly. We can type in the command **aws iam get-user**. This will get us the information on the user we configured.

##### Step 2: Creating the VPC

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-3.png?w=811)

Navigate over to VPC and click **create VPC**. Then these options;

Name – **Dev VPC**

IPv4 CIDR block – **IPv4 CIDR manual input**

IPv4 CIDR – **10.0.0.0/16**

IPv6 CIDR block – **No IPv6 CIDR block**

Tenancy – **Default**

Then click on **Create VPC**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-5.png?w=1024)

Now we are going to enable DNS host names. This will allow all resources in our VPC to have a unique host name. Host names are easier to remember than IP addresses.

When inside your VPC click on **Actions** then select **Edit VPC settings**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-4.png?w=1009)

Scroll down until you see **DNS settings** and click **Enable DNS hostnames**

Scroll down and click on **Save**

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

We are going to be creating 3 different subnets, 1 Public Subnet, 1 Private App Subnet and a Private Data Subnet.

For the **Public Subnet** we will do the following;

Name – **Public Subnet AZ1**

Availability Zone – **us-east-1a**

IPv4 subnet CIDR block – **10.0.0.0/24**

For the **Private App Subnet** we will do the following;

Name – **Private App Subnet AZ1**

Availability Zone – **us-east-1a**

IPv4 subnet CIDR block – **10.0.2.0/24**

For the **Private Data Subnet** we will do the following;

Name – **Private Data Subnet AZ1**

Availability Zone – **us-east-1a**

IPv4 subnet CIDR block – **10.0.4.0/24**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-13.png?w=1024)

Now that we have the 3 subnets created we want to make them highly available. To do this we will created 3 more subnets and add them to a different availability zone.

For the **Public Subnet** we will do the following;

Name – **Public Subnet AZ2**

Availability Zone – **us-east-1b**

IPv4 subnet CIDR block – **10.0.1.0/24**

For the **Private App Subnet** we will do the following;

Name – **Private App Subnet AZ2**

Availability Zone – **us-east-1b**

IPv4 subnet CIDR block – **10.0.3.0/24**

For the **Private Data Subnet** we will do the following;

Name – **Private Data Subnet AZ2**

Availability Zone – **us-east-1b**

IPv4 subnet CIDR block – **10.0.5.0/24**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-15.png?w=1024)

Next we are going to enable Auto-assign IP settings for the public subnets. This way any app that we have on the public subnet will automatically assigned a IPv4 address.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/screenshot-2024-08-04-150811.png?w=1024)

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

For High Availability you will want to create a NAT Gateway in each availability zone we are using. However; since created a NAT Gateway cost money I will just be creating one for this project for our private subnets. If this was a production environment you will want to create two for Fault Tolerance and High Availability.

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

_What are the difference between a Public and Private Subnet?_

- A Public Subnet routes traffic **to the internet** though an **Internet Gateway**
- A Private Subnet routes traffic **to the internet** through a **NAT Gateway**

Next we are going to be creating 3 Security Groups

- Application Load Balancer
    - It will allow traffic on port 80 and 443 from anywhere on the internet
- EICE Security Group (For our EC2 Connect Endpoint)
    - It will only allow port 22 on the outbound traffic, source is limited to VPC CIDR block
- App Server Security Group
    - It will allow traffic on port 80 and 443 from only App Load Balancer
    - It will allow traffic on port 22 only from EICE

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-34.png?w=1024)

From the VPC Dashboard navigate to **Security Groups** and click **Create security group**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-35.png?w=1024)

Name – **ALB Security Group**

Description – **Application Load Balancer Security Group**

VPC – **Dev VPC**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-36.png?w=1024)

For the inbound rules we are going to add rules for **HTTP** and **HTTPS** the source for both will be **0.0.0.0/0**

Scroll down and click **Create security group**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-37.png?w=1024)

Lets create the second security group

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-38.png?w=1024)

For the outbound we will select **SSH** and for Destination we will use the CIDR block for our VPC which is **10.0.0.0/16**

This is how we limit traffic to our VPC CIDR Block

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-39.png?w=1024)

Scroll down and click **Save**. Then lets created the final security group

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-41.png?w=1024)

Name will be **App Server Security Group**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-40.png?w=1024)

For the inbound rules we are going to add the following;

Type – **HTTP** Source – Select **ALB Security Group**

Type – **HTTPS** Source – Select **ALB Security Group**

Type – **SSH** Source – Select **EICE Security Group**

Scroll down and click **Create Security Group**

We will be using the EC2 Instance Connect Endpoint for SSH that way we do not have to manage SSH keys

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-42.png?w=1024)

From the VPC Dashboard click **Endpoints** then **Create endpoints**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-44.png?w=1024)

For name we will use **Dev EICE** then select **EC2 instance Connect**

For VPC select **Dev VPC**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-46.png?w=1024)

For Security Groups select **EICE Security Group**

Under Subnet select any Private Subnet, I am going to select **Private App Subnet AZ2**. Scroll down and click on **Create Endpoint**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-47.png?w=1024)

Next we are going to search for **EC2** in the search bar at the time. Go to **Instances** in the sidebar and click on **Launch instances**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-48.png?w=787)

For the name we will use **Test EC2** and then select **Amazon Linux**. We are going to be using the **Amazon Linux 2023 AMI**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-49.png?w=786)

For Instance type we will be using a **t2.micro** and for Key pair select **Proceed without a key pair**.

We will not be needing a key pair to SSH into our instance since we will be using the EM2 Instance Connect

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-50.png?w=980)

Click on **Edit** for the Network Settings and set the following;

VPC – **Dev VPC**

Subnet – **Private App Subnet AZ2** (It can be any private subnet)

Firewall – **App Server Security Group**

Then select **Launce Instance**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-51.png?w=945)

Under the EC2 instances click on your instance and click **Connect**

Select **Connect using EC2 Instance Connect Endpoint**

All the options should be left as default but ensure the **EC2 Instance Connect** is the one we created then click **Connect**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-52.png?w=609)

we can run the command **sudo yum update -y** to check if we have internet connection

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-53.png?w=660)

We can see we have connectivity

Next we are going to ensure we can connect to it using our PC.

Make sure you download and install [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

Open up PowerShell and type in the command:

aws ec2-instance-connect ssh –instance-id **(Your Instance ID)**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-55.png?w=372)

Under EC2 instances you will see your instance ID

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-54.png?w=1024)

First time it will show with a warning just proceed by typing **yes**

Next we will install Apache on this instance by typing in the command: **sudo yum install httpd -y**

##### Step 3: Create the Application Load Balancer

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-56.png?w=1024)

Under EC2 click on **Target groups** and select **Create target group**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-57.png?w=701)

Scroll down until you see **Target group name**;

Target group name – **Dev-TG**

VPC – **Dev VPC**

Scroll down and click **Next**, then scroll down again and click **Create target group**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-58.png?w=989)

We’re going to create another EC2 instance with all the same options as the last one expect what I tell you.

Name – **Web Server AZ1**

Subnet – Pick either **Private App Subnet AZ1** or **AZ2** it doesn’t matter

Security Group – **App Server Group**

Scroll down and click **Launch Instance**

Wait for the instance to fully initialize and pass the health checks

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-59.png?w=1024)

Under the EC2 menu go to **Target groups** and scroll down to **Registered targets** and select **Register targets**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-61.png?w=1024)

Select the EC2 instance we just created and click **Include as pending below**

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

##### Step 4:

## Switch to the root user to gain full administrative privileges

_sudo su_

## Update all installed packages to their latest versions

_yum update -y_

## Install Apache HTTP Server

_yum install -y httpd_

## Change the current working directory to the Apache web root

_cd /var/www/html_

## Install Git

_yum install git -y_

## Clone the project GitHub repository to the current directory

_git clone [https://github.com/Entree3k/host-a-static-website-on-aws.git](https://github.com/Entree3k/host-a-static-website-on-aws.git)_

## Copy all files, including hidden ones, from the cloned repository to the Apache web root

_cp -R host-a-static-website-on-aws/. /var/www/html/_

## Remove the cloned repository directory to clean up unnecessary files

_rm -rf host-a-static-website-on-aws_

## Enable the Apache HTTP Server to start automatically at system boot

_systemctl enable httpd_

## Start the Apache HTTP Server to serve web content

_systemctl start httpd_

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-69.png?w=1024)

After typing in all the commands we will test to see if the website is up an running. Head over to the **EC2** section and scroll down to **Load balancers**. Under DNS name you will see a URL, simply copy the link and past it into a new tab.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-70.png?w=1024)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-71.png?w=1024)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-72.png?w=1024)

We can see the site is up and functional.

##### Step 5: Register a Domain

We are going to head over to Route 53 and register a domain. I will not be showing that since it involves filling out sensitive data.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-73.png?w=1024)

Next we are going to create an A record, so navigate over to **Route 53** select on **Hosted zones** and click on your domain.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-74.png?w=929)

From there you will click on **Create record**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-75.png?w=1024)

We will use the following information;

Record name – **www**

Check toggle for **Alias**

Select **Application and Classic Load Balancer**

Select the region you created the load balancer in **US East (N. Virginia)** for our example

Then select the load balancer you created

Then scroll down and click **Create record**

You should now be able to navigate to your website with the URL we created

##### Step 5: Register an SSL Certificate

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

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-85.png?w=305)

When we navigate to our site we should see it is forwarded to HTTPS now

##### Step 6: Create an Auto Scaling Group

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

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-92.png?w=983)

We’re going to use the same commands as before that we used to set up our website, but we will add a **#!/bin/bash** at the top

so it should look like this

```
#!/bin/bash

# Switch to the root user to gain full administrative privileges
sudo su

# Update all installed packages to their latest versions
yum update -y

# Install Apache HTTP Server
yum install -y httpd

# Change the current working directory to the Apache web root
cd /var/www/html

# Install Git
yum install git -y

# Clone the project GitHub repository to the current directory
git clone https://github.com/aosnotes77/host-a-static-website-on-aws.git

# Copy all files, including hidden ones, from the cloned repository to the Apache web root
cp -R host-a-static-website-on-aws/. /var/www/html/

# Remove the cloned repository directory to clean up unnecessary files
rm -rf host-a-static-website-on-aws

# Enable the Apache HTTP Server to start automatically at system boot
systemctl enable httpd 

# Start the Apache HTTP Server to serve web content
systemctl start httpd
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