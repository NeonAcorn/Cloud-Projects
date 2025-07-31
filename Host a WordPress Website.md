![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcXtv1JdZBBU3hB73YBunC4PbnEV-hOaIyP9a4Th2Rj_THNEphZb4ivpMD4JLB0Q_7Y4gygXv4Mr3Qa7I6qn3FfI_89hZ0ZaMVUM8yf4I0Mm6twHGMZ9hIoQV0Xb20wRxoXDMhH?key=lPrS3P9tXZMjc9bmE4-JEg)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc8A94Fpt22LT1hafUwiJoEvzq1zU3ybzlSfKFoTQXOguRJTNR7FJ4-jzdL4BpilQ5OutG8BM7QHRB4Sn7iV-IRcGswDWxzm26LOEOiRjP6n58r-r2nJ6zOjan6PQfNY3Y46mc0XQ?key=lPrS3P9tXZMjc9bmE4-JEg)
## Step 1: Creating the VPC

Navigate over to VPC and click create VPC. Then these options;
Name – Dev VPC
IPv4 CIDR block – IPv4 CIDR manual input
IPv4 CIDR – 10.0.0.0/16
IPv6 CIDR block – No IPv6 CIDR block
Tenancy – Default
Then click on Create VPC

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfgkNNGbVfGe2QDk6-jJHzx40tor_af3DUInck_Vykrm-b40eRdm9123rc88Z6ELl5eta9467PvmtMpLemXhBqKGqFetFLO65AkI8Px3U26IAaRSefpgEYSRPYB_KJW3g40imaZsw?key=lPrS3P9tXZMjc9bmE4-JEg)

Now we are going to enable DNS host names. This will allow all resources in our VPC to have a unique host name. Host names are easier to remember than IP addresses.
When inside your VPC click on Actions then select Edit VPC settings

Scroll down until you see DNS settings and click **Enable DNS hostnames**
Scroll down and click on **Save**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeG1fs30hG2VZREdTqlzZlekz0WcVfLOzqOYAXwMPFhtAQmXqvlMuJm1k9WCuN6VYoVR1XlS4V5suyaxWbSyo8S28oYuwStxQiDR7djsUk2vvzdD3C_ziGtTSbhzMKQUFtz91z5?key=lPrS3P9tXZMjc9bmE4-JEg)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcTCEQoftBjMmHsSGFh1z1dwjq5E1FT-Nn0Zj61JXma-dJGYHTEqAkNwJWm-U9JAMC9ZzWN6mNGlBuncHaXF-kXXMuMZ8KDsiklLgmWBh9Rfi6S4CU-YfI5ekYgud9x9a59aA0i9A?key=lPrS3P9tXZMjc9bmE4-JEg)

Now we are going to add an intent gateway to our VPC. When on our VPC click on Internet gateways in the side bar to the left. You can only have 1 Internet gateway for 1 VPC. To make sure you add the Internet gateway to the correct VPC click on the box labeled Filter by VPC and select the one named Dev VPC and finally click on **Create Internet gateway**

From there we are going to name our internet gateway to Dev Internet Gateway and click on Create internet gateway

Now we need to attach this Internet gateway to our VPC, so click on **Actions** and **Attach to VPC**

From there click on the box labeled Select a VPC. This will only display VPC that do not have an internet gateway attached. Click on our VPC named Dev VPC and click Attach internet gateway

We need are going to create some subnets. Click on Subnets in the menu on the left. Then click on **Create subnet**.

We are going to select the **Dev VPC**

For the Public Subnet we will do the following;
Name - Public Subnet AZ1

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc9AFsJzpRrTNFQq5w499ffVn9uCfJWkj1-umx495d9GCzBHSHLHF_4uNLI-3EYCVBuAY9iOV7KHHuKomvwHnvoMs8rh7WGTp_zmYP8-3NCuTg56PBtcrLwQ3wcRFZ-BO4LgFs48A?key=lPrS3P9tXZMjc9bmE4-JEg)

Availability Zone - us-east-1a

IPv4 subnet CIDR block - 10.0.0.0/24
For the Private App Subnet we will do the following;
Name - Private App Subnet AZ1

Availability Zone - us-east-1a

IPv4 subnet CIDR block - 10.0.2.0/24
For the Private Data Subnet we will do the following;
Name - Private Data Subnet AZ1

Availability Zone - us-east-1a

IPv4 subnet CIDR block - 10.0.4.0/24
Now that we have the 3 subnets created we want to make them highly available. To do this we will created 3 more subnets and add them to a different availability zone.
For the Public Subnet we will do the following;
Name - Public Subnet AZ2

Availability Zone - us-east-1b

IPv4 subnet CIDR block - 10.0.1.0/24
For the Private App Subnet we will do the following;
Name - Private App Subnet AZ2

Availability Zone - us-east-1b

IPv4 subnet CIDR block - 10.0.3.0/24
For the Private Data Subnet we will do the following;
Name - Private Data Subnet AZ2

Availability Zone - us-east-1b

IPv4 subnet CIDR block - 10.0.5.0/24

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcpwsLBdS3PSvMBhk1Lx7IvlLDibBWqiXl9HI4ZV69tffP5SPbZCb_cs0z1z50K7xOmNJg8C5KJDoJFWwgRpFFz9s8dTYxqiLs5oyJiJbjuDlOZJL12rBVypMLE_Mx89MU1NhDq4A?key=lPrS3P9tXZMjc9bmE4-JEg)

Next we are going to enable Auto-assign IP settings for the public subnets. This way any app that we have on the public subnet will automatically assigned a IPv4 address.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdSt3RSXhf5dk7MmDx1z1kqT8KESDUW3t6SknlOOYUZrjy29X8mo_T4c7cK13rHlIj6fP-xYCsXZ4GHeFcDAQX9xOZ1NLRih-5yDlWzXRcAJKHyp0leilvg2VI0tKuEoQhjClW-?key=lPrS3P9tXZMjc9bmE4-JEg)

Select **Public Subnet AZ1** then click on Actions and select **Edit subnet settings**.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdSt3RSXhf5dk7MmDx1z1kqT8KESDUW3t6SknlOOYUZrjy29X8mo_T4c7cK13rHlIj6fP-xYCsXZ4GHeFcDAQX9xOZ1NLRih-5yDlWzXRcAJKHyp0leilvg2VI0tKuEoQhjClW-?key=lPrS3P9tXZMjc9bmE4-JEg)
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXebazo55VjyAPirw8u4eTW7gsKuQ9TUa8g4NHwMLGiL6ivCjb-PbB1BXG5RbreEtWDNl-9GPBwYSXqE9X8FymcFCqEQedbjnPW_Ye6Mh1u8bgOvGnQRNMNhzEF6xZ5GEUCnfatPkA?key=lPrS3P9tXZMjc9bmE4-JEg)

Under Auto-assign IP settings click **Enable auto-assign public IPv4 address**. Scroll down and click on Save. Repeat this same process for the other public subnet Public Subnet AZ2.
Next we need to make our Public Subnets public. Just naming them public does not make it so they’re public. To do this we need to create a routing table.

Under the VPC dashboard click on Route tables then select **Create route table**

We are going to name it Public Route Table. Then select the VPC Dev VPC and click on the Create route table button.

You can see we already have a route created by default and this is a local route, but we need to make sure we create a route that can get to the internet. Click on the Edit routes button.

Click on Add route in networking if we want something to get onto the internet the destination will just be 0.0.0.0/0. For the target select Internet Gateway, then click the box with igw- and select **Dev Internet Gateway**. Then click on the Save Changes button.

We have created a new route. Now any subnet that we associate with this route table will be public.

To associate the subnets with this route we click on Subnet associations then click on Edit subnet associations and select the two public subnets we created.

We do not have to do anything to make the private subnets private. If you look at the route tables we will see we have two of them. The one we just made and the one that is made by default when creating a VPC. The default one is the main route table.
You can see on our Public Route Table we created we have 2 subnets that are Explicit subnet associations. What this means is that we explicitly associated those two subnets to that route table. However, since we have not explicitly associated our other subnets that will be under the main route table by default. And the main route table by default is private.

We can verify this buy selecting the route and clicking on route as we can see it only has a local route and no routes that are going to the internet.

Under Subnet associations we can see that our other private subnets are not under explicit association but they are under Subnets without explicit associations. AWS explains that this means “The following subnets have not been explicitly associated with any route tables and are therefore associated with the main route table“
Next we are going to setup the NAT Gateway. The NAT Gateway allows for the resources on a subnet to have access to the internet without allowing the internet access to the resources in your private subnet. Which is useful for example an EC2 instance connecting to the internet to download an update.

In the VPC Dashboard select NAT Gateway and click on **Create NAT gateway**.
One thing to always remember when creating a NAT Gateway is that they always go in the Public Subnet. So I am going to select Public Subnet AZ1 but you could Select Public Subnet AZ2 or use both if you are going to create two NAT Gateways for HA and Fault Tolerance.

We are going to do the following options;
NAT Gateway – Nat Gateway AZ1
Subnet – Public Subnet AZ1
Connectivity type – Public
Elastic IP allocation ID – Click on Allocate Elastic IP
(We are clicking on that because we do not have any Elastic IP allocations)
Then click the Create NAT Gateway button

Head back to the Route tables section in the VPC Dashboard and click **Create route table**

Name it Private Route Table and select our VPC Dev VPC and click on **Create route table**

Under the Routes tab click on Edit routes

We will click on Add route and add the following options;
Destination – 0.0.0.0/0
Target – NAT Gateway
nat- Nat Gateway AZ1
Click on Save Changes

Click on Subnet associations and click Edit subnet associations

Select all the private subnets and click **Save**
Next we are going to be creating 5 Security Groups
Application Load Balancer

It will allow inbound traffic on **port 80 and 443** from anywhere on the internet
EICE Security Group (For our EC2 Connect Endpoint)

It will only allow port 22 on the outbound traffic, source is limited to VPC CIDR block
App Server Security Group

It will allow inbound traffic on **port 80 and 443** from only **App Load Balancer Security Group**

It will allow inbound traffic on **port 22** only from **EICE Security Group**
Database Security Group

It will allow inbound traffic **mysql port 3306** from the **App Server Security Group**
EFS Security Group


It will allow inbound traffic **NFS port 2049** from the **App Server Security Group**

It will allow inbound traffic on **ssh port 22** only from **EICE Security Group**
After you make the EFS Security Group go back in and edit the inbound rules and add another rule that will
It will allow inbound traffic NFS port 2049 from the EFS Security Group
The reason why we need to allow the security group to have access to itself is because multiple instances will be directly accessing the EFS at the same time and to access whatever information we have on it and in order to do that successfully we need the EFS to allow traffic from itself
We will be using the EC2 Instance Connect Endpoint for SSH that way we do not have to manage SSH keys

From the VPC Dashboard click Endpoints then **Create endpoints**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdgDkrob33o5puNOSieyNWAlKug6zK6_g0Hx1ARpKyJ1HV7UUIvCi7aziWkmt6YZXM_WHqKn_vgHZw55PddnKisSYpaN2GiVH2oVIDCFgbNKFf2veM601xV76o3ihHIcU88NAVKgA?key=lPrS3P9tXZMjc9bmE4-JEg)

For name we will use Dev EICE then select **EC2 instance Connect**
For VPC select **Dev VPC**

For Security Groups select EICE Security Group
Under Subnet select any Private Subnet, I am going to select Private App Subnet AZ2. Scroll down and click on Create Endpoint
The Elastic File system allow for multiple servers to access the application code from the same storage. EBS only can be attached to one server at a time so we would need to have multiple EBS volumes for multiple servers. Which mean each change we make on one instance will not be reflected on the other instance automatically

Navigate over to the EFS page and click Create file system then select Customize

Name it **Dev EFS**, leave all options as default then at the bottom click Next

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdGLmtUh50mFtKYbXRJRW7h9bwv4spk0rBxlEEunsYnf-9ZIIY8oDi031O4_ze7hZ2f_eaWyleE8u-D8MALkTpeD3vbu_uimF4QXbooz7-u2AcM-4iAxd4sF3sJpSoT91hKrXzYSg?key=lPrS3P9tXZMjc9bmE4-JEg)

Change the VPC to **Dev VPC** under **Subnet ID** change both AZs to the Private Data Subnet option. For Security Groups set them both to **EFS Security Group** then click **Next** and **Next** again then review and click **Create**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfLY653A_kJgdMcTKDQkqu6-lzySZsgc4QyNjEIC17sZDbzqoPrEZ_xxNsoYI1Q0JiryfdZE1vISZ4-gJjIfYGmTeB5TvbTkMMl_OOrjGTiyzliV_5V4xgsa9iflpXFktxt6P9w6g?key=lPrS3P9tXZMjc9bmE4-JEg)

Navigate over to RDS and select Subnet groups then click Create **DB subnet group**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd54qrO14ZKYVDc_M4ZKdwyR29Pt115mviROX1ocWLuHx7WlYW-eRCnGWL0Ng0l0joQPbrSEi1Q1qfIv6oMN0jAoVkiI8T6X4tpv4Pck16eXCdaJNNBwcYjRA9jwQP3hHlKGQ-4?key=lPrS3P9tXZMjc9bmE4-JEg)
![AD_4nXdAkd0XbG8oo0s3_28rcQLdKRPCQ7iQUVY6QwDABg1zVoo6LYD47OBm_qZq07fpbxz3bSXlV2s7xJRQ1ScpGWtkjeMWvBldrL_WXRqigg4vOAn-ydZK_S6pkgkgCowPiwR7-wr1?key=lPrS3P9tXZMjc9bmE4-JEg](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdAkd0XbG8oo0s3_28rcQLdKRPCQ7iQUVY6QwDABg1zVoo6LYD47OBm_qZq07fpbxz3bSXlV2s7xJRQ1ScpGWtkjeMWvBldrL_WXRqigg4vOAn-ydZK_S6pkgkgCowPiwR7-wr1?key=lPrS3P9tXZMjc9bmE4-JEg)
Select **us-east-1a** and **us-east-1b** since those are the AZs we have resources in.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeYpGlLUY4OXqcPtsKoMRuB0syLvoV0eni9iNfEJyf8eAlTHn5pUhAQ_MAZFnIgx2Ro9zVcDCnnftr0cTeAov4xzinzg1uf1DVRhhPAPEo5SUUXHKWsb1_5OrFPOC9U9XcRebfLbQ?key=lPrS3P9tXZMjc9bmE4-JEg)

We are selecting the subnets for **Private Data Subnet** and we can verify these are the right CIDRs by heading back to our VPC.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXelc3QSDuKd6OndYTec7R954I3pWp2TWT9AyX8ez3dnqxXM6J_cze1O2Z7YBxBeg1Q6jhiLErOunGsXv0CHmnOLYXZfnL_KG5OVjIgH7poYmvDYWuT5wEAZRhLvAPhZjxGMM6eSiw?key=lPrS3P9tXZMjc9bmE4-JEg)

Navigate to Databases under RDS and select **Create database**
then select these following options;
1. Standard create
2. MySQL
3. Engine Version - Latest Version
4. Single DB instance
5. DB instance identifier - dev-rds-db
6. Master username - Enter a username
7. Self managed
8. Master password - Enter a password

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdtsqtrjm7lSqrsoc1SmUXPkIIh2NeWmo92SuIvHW4Vkvkikuc6MB_XDMOQOHqhqOhOfZqFmXyRw4RCVE_nOzfJN3aKRfPWLdBeOTSz1hMRB5oTwWe2rWU21-lafssdtfcTTJv3LQ?key=lPrS3P9tXZMjc9bmE4-JEg)
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcxjMxxByLEhtKFKocEnlYURUElCQXZo4jNk-smjtROQjhDa_C3JYi1Ajk1DF3ABrimjNIMNRl7_BnJommkZfatB20rg9Nz-tG67kBKoozNnmLxjfHdYuYknKnoW79qEvFlicYKfQ?key=lPrS3P9tXZMjc9bmE4-JEg)
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcEDtVOC32a2fEdR0_XF_aYb-GlesBXbdY46136cSWvbVQSxWNdL-TSxgmkRwFJs_JHb8C6yOMT5aol8mlfCBCFndVSRgMIuXzWX-dE-gUsVQ4TSxhE1pcK5xkSHOqievQPh7zKBQ?key=lPrS3P9tXZMjc9bmE4-JEg)
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfyPI312E1PV4_LQdk6GJ6Tw38h9Mo354p7pqJ59ZEx6byR2dnVt8HK7KT9iZ6UxQKZFl-5NGDI1jXndMq4cuEZhB3VWMhUzxnxX8KtyzMhbzZeJG15cUCm4_KrCyJSdjOPRQhn7Q?key=lPrS3P9tXZMjc9bmE4-JEg)

Scroll down and click on **Create database**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdMOnruqE5TCwc-i72iFexSl1qoQC8EYM7JcaanxhtBj1OtaukaxKfEqs1ionOP3LAwWp-gpLAkh6VAl3trgl5_cQkcoFSCHECyo-dUMoufJrNZUN2JyGJ23WkJ4oEVvdw6wI_y?key=lPrS3P9tXZMjc9bmE4-JEg)

Under EC2 click on **Target groups** and select **Create target group**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc1iEeGfSz4maUe0qBK5j_8aH415nPdLUO65O0Go8MsT0D6DPRymfU8nJ-0_1Y6oJwQlf8NETUUZTVWW6jY5L1UzfirHztUrfk_CQUx0Dk07ByJyvx3IXM3z2h_ozuX3a8M1CStUQ?key=lPrS3P9tXZMjc9bmE4-JEg)

Scroll down and click Next, then scroll down again and click Create target group

Name – **Web Server AZ1**
Subnet – **Pick either Private App Subnet AZ1 or AZ2 it doesn’t matter**
Security Group – **App Server Group**
Scroll down and click **Launch Instance**
Wait for the instance to fully initialize and pass the health checks

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc4YQ035Oa_XFmpRB7Q-kzKekLDaDSedla4cmhMQFkrlJhKceIcq4f47DY6dVNNGaSZ8CjEAd6b7S0wcBnguobEJP0YE19SZl8SVq32aFy0G7_D-iPwSyuATHylVBchwZTsO6Xh?key=lPrS3P9tXZMjc9bmE4-JEg)

Under the EC2 menu go to Target groups and scroll down to Registered targets and select Register targets

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdxdv7G-ILGR0s7BC7Sf9-2vwvsS2AAVxC0ebC2FM8w_kWMhVwgF7aEP-a6_FZxmpvXeMTIjAlZH1JZYRK6uE0funaj37EuO55wEDJK3W3FEubC2_zOsiMuQcFCxCtg-AJJ23oKJA?key=lPrS3P9tXZMjc9bmE4-JEg)

Verify it is under the Review targets tab and click on Register pending targets

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf9aRsMS9iCdPUI5feOuQRGMO3gNDP38DSCttfmLThFIR3Hiv40C4ouWcW45yAGcYj0KV9NZfd7rcD4n6a-OPJAyKQcJIXz3oRO4u2P9MeMMJiC5JEXc_kgzpn1CF5DP3FNyQqk?key=lPrS3P9tXZMjc9bmE4-JEg)

Next we are going to create the application load balancer. Under the EC2 tab scroll down and select Load balancer then click Create load balancer

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXegbYGgZ1fz1WsAa8kpxvkEenQvu2WlRNiE1eWIgBFMU2OtfviWDec-EPkpNmUnKPdVoJyIokg6vIG_9FI4Yj3g6R6B_yTBnRM6nplpyom-2AA7JQF7S7S9bkvHCfyuicohmzDkpA?key=lPrS3P9tXZMjc9bmE4-JEg)

Scroll down and click **Create** under **Application Load Balancer**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXci0BPv8m6RPoyk4Su9r_-XULnFnjsbFYLI_eAkI_fR1AuDQIdsFRp5S0q1mYbaCri6cZK0Fumc0SPEjI6ihc1u-RLbVlNpH9od7DSdyWHpiiPvG0zAj618J0hJvsQFt4_IJRifpg?key=lPrS3P9tXZMjc9bmE4-JEg)

For the name give it **Dev-ALB**, the rest should be as shown

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeYAJK7ME4O2IprqYcyUIOoawUDnJ4lC4HIRXGW3tw1MsmyaXNwKJrUIr0JaYttLwoNkKFimqmbKbPIsg8cl-092LYdmPF5zC7pv_1GwC6sD7dQm5JNvaYAuRZsFfe8c7C5s3KEbA?key=lPrS3P9tXZMjc9bmE4-JEg)

Select the **Dev VPC**
For Availability Zones under Mappings we will check both of the AZs that we have subnets in. For an Application Load Balancer it will only work on a Public Subnet so lets select **Public Subnet AZ1** and **Public Subnet AZ2**

For the Security groups click the X on the default security group that is selected and then click the drop down and select ALB Security Group

For Listeners and routing select **Dev-TG** in Default action.
For now we are only going to create an **HTTP listener**. When we register an SSL Certificate we will create an **HTTPS Listener** to redirect our HTTP traffic to HTTPS
Scroll down and click **Create load balancer**

Navigate back to the EC2 page and connect to the Web Server AZ1 instance

Enter the root user
```bash
sudo su
```

Update the software packages on the ec2 instance
```bash
sudo yum update -y
```

Create an html directory
```bash
sudo mkdir -p /var/www/html
```

Environment variable
```bash
EFS_DNS_NAME=fs-064e9505819af10a4.efs.us-east-1.amazonaws.com
```

Mount the efs to the html directory
```bash
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport "$EFS_DNS_NAME":/ /var/www/html
```

Install the apache web server, enable it to start on boot, and then start the server immediately
```bash
sudo yum install -y httpd
sudo systemctl enable httpd
sudo systemctl start httpd
```

Install php 8 along with several necessary extensions for wordpress to run
```bash
sudo dnf install -y \
php \
php-cli \
php-cgi \
php-curl \
php-mbstring \
php-gd \
php-mysqlnd \
php-gettext \
php-json \
php-xml \
php-fpm \
php-intl \
php-zip \
php-bcmath \
php-ctype \
php-fileinfo \
php-openssl \
php-pdo \
php-tokenizer
```

Install the mysql version 8 community repository
```bash
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm
```

Install the mysql server
```bash
sudo dnf install -y mysql80-community-release-el9-1.noarch.rpm  
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
sudo dnf repolist enabled | grep "mysql.*-community.*"
sudo dnf install -y mysql-community-server
```

Start and enable the mysql server
```bash
sudo systemctl start mysqld
sudo systemctl enable mysqld
```

Set permissions
```bash
sudo usermod -a -G apache ec2-user  
sudo chown -R ec2-user:apache /var/www  
sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \; 
sudo find /var/www -type f -exec sudo chmod 0664 {} \;    
chown apache:apache -R /var/www/html
```

Download wordpress files
```bash
wget https://wordpress.org/latest.tar.gz   
tar -xzf latest.tar.gz  
sudo cp -r wordpress/* /var/www/html/
```

Create the wp-config.php file
```bash
sudo cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
```

Edit the wp-config.php file
```bash
sudo vi /var/www/html/wp-config.php
```

When you run sudo vi /var/www/html/wp-config.php an editor will open up. Press i which will change the mode to insert

Use the arrow keys to navigate you will see the options
**database_name_here** – The name of the database you gave it
**username-here** – The username you made
**password here** – The password you set
**Local_host** – The RDS endpoint

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfNqDuQnTakJRkY9tk9T40-qEJuIuHX35TJzhm2AvywggLk-SssJbWMu0pNZOVVB4_qUPGiexjS5IfDQYMzQVzZd1tU6pxHS5oSuqukL2ifgWgwTnJVXoZGrv6jwBphyVl9WMY-YQ?key=lPrS3P9tXZMjc9bmE4-JEg)

The RDS endpoint can be found under the RDS database tab Connectivity and Security
Once you have all the details entered press esc on the keyboard and type :wq! which mean write quit

Restart the webserver
```bash
sudo service httpd restart
```

Run this command after writing the file

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdzB5b9SNkKfhLNEtGp_abDpnxUQ1a4YfQ63WqGSKIZa1q5NXp0cxGzNTHgHpPAtQkJ954tNTcP8ZIUYhFNOLhaUozdA7UcB355wD-Tw2OcCyYRK6YdziapP9olREOxRzGXzaEUOA?key=lPrS3P9tXZMjc9bmE4-JEg)

Under EC2 go to load balancers and copy the DNS name into your bowser

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXekDHaxbur7iRrFpy0CtkMv3lDFF2Z8N8yrPBxVUocPTqtQEE6yxxF9XCGuOKcxDDvXkCoD96TpiZrg7-uXyNMPUikfWdnGpc-MUnYyn4wu_4xWRyQjYMtkFT7-JGoz3LHvUFhnzg?key=lPrS3P9tXZMjc9bmE4-JEg)

Fill out the information to whatever you want

We are going to head over to Route 53 and register a domain. I will not be showing that since it involves filling out sensitive data.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfBkcvi27lbHmHOaNBgpIwbVV0imx0eqOrAKu-lRmnmuaVE7XEywI4mOUeOHIRCCQPCrjTQjaH0xX4DJj-SAJs22q8rcPLLm0fRIiXRSRX_l9zS2LHJXW-2bpF1riSkFRLRrP_2vA?key=lPrS3P9tXZMjc9bmE4-JEg)

Next we are going to create an A record, so navigate over to Route 53 select on Hosted zones and click on your domain.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfKlbH4bYI_B3z8haVlqn3CHvHM-EAFATOBWkZHmF29Vr8OhSucN8u842GMdbxJojYJwwreT-K35z_dGs0QXCuJZ8MxHAiYDvv2lwHtLjh8oQaoCp6-oqZufP63BYS2uIU7C8HgCQ?key=lPrS3P9tXZMjc9bmE4-JEg)

From there you will click on **Create record**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcVCS7331LIBSpuqAVUcUsTwxD5j-mZ63Dyx9UhOQIn2ChUVNJ2IbmSa8XI9NOvGcKI6MVXAstl-dPbG_LrXZwtGQGm9tGoQWDwb6a6xQTq0WzL1hUexxnWz2aY78O8yTa5kOne?key=lPrS3P9tXZMjc9bmE4-JEg)

We will use the following information
Then scroll down and click **Create record**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcNw9jLdjjnjtbgRsh3MBZ_97XTqq-YvfZXn0uyHpKjrC4_y5Oj5qHrQd2_srJdgLPAPrlhzO5Z5Bs2yjOAbgheB2-ALiIFRIq9aYYq-XDppommTy_43fYXG4pYBLOQ9EEjAQ9WcA?key=lPrS3P9tXZMjc9bmE4-JEg)

You should now be able to navigate to our Wordpress site.
Now lets update our domain in the Wordpress site go to http://www.yourdomain.com/wp-admin

Got to settings -> general

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXecps7ifL8KRMkgJ2nI5dj4Vn6yTMH6f8Da1_kpxUkErRt-9XAX3zAXf2PWZGZ0BHHM5_AYGriIyf1eKKo76otFZXWwxNbLaiYi9LojyjuL5IWdDebV4FpKl8XoyfbzhzYproMQ?key=lPrS3P9tXZMjc9bmE4-JEg)

Let’s up date the URL to our domain

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcsHRG34F4isbRNyoh7RsWWlOtJKWIVIzMNWYZIi4ejO1Dlxc6wTjlebhUfP_0XpDllalegdU0bvFAWs1uoZeIYDLBTl645Ux9FmJNgB43uR2NlS__vJ-QR8uhKHF4G7xVXVpv8mQ?key=lPrS3P9tXZMjc9bmE4-JEg)

Navigate to Certificate Manager by using the search bar and click on Request a certificate
Select Request a public certificate then click on Next

Enter your domain name then select **Add another name to this certificate**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdpTJwVE7HjPUPEiMm8TwJja4RlABga0WhmfeHMTzcPtlCUV9MxrXcOah0uib-LPqNzZ3hnKJn1CsZ6F04_zKxSEDJTPysEMIy-xUC122MIRO-zgxOqK599fM8cUm0PdbOWWecRMg?key=lPrS3P9tXZMjc9bmE4-JEg)

Now we are going to add a wildcard for our domain so enter *. followed by your domain name
Scroll down and click on **Create**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXek5M2VFQlFCLnv5EvAS5twvj9t9586abyBgt2mIYwxmpBHSvO94r8WisyaRnUi3kDHQmMD_XVzrkMQaFfMBsKaeUnxP8V9Q1FPMz-1wi9GxXaJIqkhSyx-IT7MF3mtRNp3zefQ?key=lPrS3P9tXZMjc9bmE4-JEg)

Under Domains click on **Create in Route 53** then select the two domains we have created

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdsnBvkQq3hC-UXtCuGs0g_4L2u0cvfxEWD3dQI-HyLhIitkqiGp6ELAcC366PX7Xc4D66FOCL4AVFFXff9398gWFm41HHpQ5_QeQhHytdecPK1MA9n35lbWXFWWKJV5grZPsGInw?key=lPrS3P9tXZMjc9bmE4-JEg)

Navigate back to EC2 then load balancers and select your load balancer

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXceO-S280pisevaBfXvYkrYvYwGVZ9DYZ3RxfCCUGxakLua1Lana5B3KzF5NCKrX2Yso-aY8d2iFfaPPiLsFb3VRb3DCKs6xeS6D1aJeHoaOtTPq_UAKNLglJaK1XP38yGbDCzokw?key=lPrS3P9tXZMjc9bmE4-JEg)

Under the tab Listeners and rules click **Add listener**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdj5S-21fCTK7XjrnEl46vGbiMjRySUFncDirKkP0dkIDQ1wJwlh7Ih-a6vo0l1CJZwhAMY7IxJc3BxhaReiH4nNmj5FxF7ZaSeWQKNyG4bliR2DvyJdJSYkrlloH_LEdSV1cwS1g?key=lPrS3P9tXZMjc9bmE4-JEg)

Set the protocol to **HTTPS** then under Routing actions select **Forward to target groups** and select your target group from the drop down

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe2J1pelvjfO44I3_eIcLd4DNfCXkWq4tsh4mnAyImP0VwYyqSrS1_fokjbW4hiB9b_fB9A9-0a_H6mwK2OaZxnzAzAMliYGn73x9hH81fOkbUzrmxLE5hV8gb6jIt5VgUK3QG6hw?key=lPrS3P9tXZMjc9bmE4-JEg)

For Certificate source select From ACM since we registered the certificate with Amazon Certification Manager

Head back to the load balancer and edit the listening for HTTP port 80

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcy9vMohtKYRJcObApoF0-h1ZNHkH7ZzcK7ZVZeqpqVIGhHGvf4DfwY8lVvB7HgzkmlNwQ_kKbkMnIg3qVdmStqy23jEO_zrh1fKcc8W95wpbs_M1D7sztDhQEEYs4vLWwCyqeV?key=lPrS3P9tXZMjc9bmE4-JEg)

We will change the Routing actions to Redirect to URL then select Full URL
Scroll down and click **Save**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfOCzq5AH-1uKThjTv5K7KtIwdhRMCTfeYYXUkSyF7LV1HJiN5OR4Rowg-kqBOnUGvQ0G8fonq9ZfkuXigsOyJ03QLrcIsbDX_n1kWpqBHJVKiDwWNbT_8iXIQO8CxdNLvkoKc2?key=lPrS3P9tXZMjc9bmE4-JEg)

When we navigate to our site we should see it is forwarded to HTTPS now
We want out Auto Scaling group to dynamically create our EC2 instance so first head to the EC2 instance we created and change the instance state to Terminate

From the EC2 menu select Launch template and click on **Create launch template**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfBnQGR_CN5WF4E3LaZNZEdIrh29RIj5CoiIQZnDp-5DCVYi9GKtaIs0aRmW13xbpcJi_RQ_FH_dBOZqWQgP1OQgy-bfqXFPq94WAgyhH04xQ0PH_MGDiFgey0047FnAoI-i9USzA?key=lPrS3P9tXZMjc9bmE4-JEg)

Name it whatever you want Dev-Launch-Template and check the box for Auto Scaling guidance

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcXHB8EA4q8ZWZ-AW1mkIt0gFTIhD-53h1sU2vikJseFFEGYmWRZH8TfNXloQQOhb3prktBWvorNI5RPuLXdyjB9O4WmL2p6-8GHb59c0gy-PjtutNUE4_50x7MaenMU80XjQNZ?key=lPrS3P9tXZMjc9bmE4-JEg)

Select **Quick Start** and click **Amazon Linux**

Select the **t2.micro**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcdmp7TwndVsRV65bryFohl4fBz4enSBAqGcY4GOcotyUfBFk2M7yWsSlQfYTUoCuG_AQJvxoLE11Oj8Jxo7WSQV4rLXBKy9UgG3cZMuIfza4h1WzGX5WPICS0CzAGWR0pUZof4KQ?key=lPrS3P9tXZMjc9bmE4-JEg)

For the security group select the **App Sever Security Group** we created
Scroll down and click the arrow for **Advanced details** then scroll down to User data – optional
so it should look like this
```bash
#!/bin/bash   

# update the software packages on the ec2 instance  
sudo yum update -y   
  
# install the apache web server, enable it to start on boot, and then start the server immediately
  
sudo yum install -y httpd
sudo systemctl enable httpd 
sudo systemctl start httpd
  
# install php 8 along with several necessary extensions for wordpress to run
   
sudo dnf install -y \
   
php \
php-cli \
php-cgi \
php-curl \
php-mbstring \
php-gd \
php-mysqlnd \
php-gettext \
php-json \
php-xml \
php-fpm \
php-intl \
php-zip \
php-bcmath \
php-ctype \
php-fileinfo \
php-openssl \
php-pdo \
php-tokenizer
  
# install the mysql version 8 community repository  
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm   
 
# install the mysql server
sudo dnf install -y mysql80-community-release-el9-1.noarch.rpm  
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
sudo dnf repolist enabled | grep "mysql.*-community.*"  
sudo dnf install -y mysql-community-server 
  
# start and enable the mysql server   
sudo systemctl start mysqld
sudo systemctl enable mysqld
  
# environment variable
EFS_DNS_NAME=fs-0ad7db433467262ad.efs.us-east-1.amazonaws.com

# mount the efs to the html directory 
echo "$EFS_DNS_NAME:/ /var/www/html nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2 0 0" >> /etc/fstab
  
mount -a
  
# set permissions
chown apache:apache -R /var/www/html
  
# restart the webserver
sudo service httpd restart
```


Then click **Create launch template**

Under the EC2 instance select **Auto Scaling Group** and click **Create Auto Scaling group**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeom5I3tFhN7XM6ziijatzRAbyvr1ntrE5RcyxxSwEzh55gOoJptjYWlt22vHefbW9i-7dOdTBhlqisEF3o-BSfVbKw4tUhG9Q1Fjbvi9dREYqIxzg1et3Q2eqlcPcg6u7M6YIG?key=lPrS3P9tXZMjc9bmE4-JEg)

For the name I will use **Dev-ASG** and for Launch template I will select the one we just created. Scroll down and click **Next**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfth8GWFB-0l4tYO2MtcZXaEaQTk4pawTrHsKoR-H8aJJWfTPjpZuqbj-GCf4E6AG3blyE1kN14k3rhWI9ZelhaBKY1hanFt1VCbXc24BNP5e3FoShMz5xTtipyDPXAAHhj-hBX?key=lPrS3P9tXZMjc9bmE4-JEg)

Scroll down to Network and select your VPC **DEV VPC** and for the availability Zones select the two **Private App Subnets AZ1 and AZ2** that we created click on **Next**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdn2BqXYsW7GNw6L6Y8q8dDAx9ml8ix3LdUmfRu_TYYGcLB-NzF4lg_lAG8Ia4ecjzUsojslb9BJUJ4vDdV8M7yPl933V5CHgUf2Iac28TCiMrxTd1q_x4PNgAlBeTDUgqfdDWBDg?key=lPrS3P9tXZMjc9bmE4-JEg)

Select **Attach to an existing load balancer** then select our Application Load Balancer we created

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdrcHfLUWNxS1gQLXSac5yuOkLEnrhdwxkqrJnRdzfkbNc0Yj4Z0syqFUefvi5yTjABm1RG7fvNa6UQBrwu7daCqAnEZG51RcSLWuTBdfip-RPOQoqUIIFCwsMzw2BKvol1KKqeHg?key=lPrS3P9tXZMjc9bmE4-JEg)

Check the box for Turn on **Elastic Load Balancing health checks** scroll down and click **Next**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeYyebyJ6cWZy6SAKDghE9FIhfI1UTCtbDYaD8xu8y9r6rKRA3b_vTEDR32HvcTbyygNmzd2VFn6vY1uR5ZzYfww6Jd2B0NS5hmKzC2uF5U08lOt2R8eWRsIDOpB7BbY11jygBciw?key=lPrS3P9tXZMjc9bmE4-JEg)

For Desired capacity changed it to **2**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfKdKGLIcEpPprZcX3SW6VkCJmTxSgSVSlZ0DpDUKKpaREAEfmnLzk4wzsGvfK6JivY1tsmnYXVFS-wY8KweeaWqOEBO7aiLdJJFtdiiYdvRkGe2GgCoqJjANEQ9jKqX46L5mf4Gg?key=lPrS3P9tXZMjc9bmE4-JEg)

For Scaling enter **1** for the Min and **3** for the Max.

What this means is that minimum capacity we will scale to is one EC2 instance and the maximum capacity we will scale to will be three EC2 instances.

Keep the **Instance maintenance policy** set to **No Policy scroll** down and click **Next**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc2XBqy2ciciOca3sdUgycKdeZIQQkknhxSJcxgCsf6AA-cj375zSGgOIxBC_AITkC_5gb9H4_JDJ_5lw5WfMBZHuDWKx0x4dTG9_SZaieq_TPSlbe7UX_pPLE13YCwcHIjgXqB?key=lPrS3P9tXZMjc9bmE4-JEg)

If you would like to add a notification for the EC2 auto scaling group click **Add notification** then select **Create topic**. Give it a name and enter the email address you want to get the notifications. Scroll down then click on **Next**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdlAnnnVWBFicJYnXdORSwRBBFpb0pcJrOVjluc9LGh3CStqWdoTeQpelchK77zsQFQURf_1kp1mLUV1Hkf6Z3MSU4RxMO1G-BFI7CDdMnuHlXt6odtPEImIaegYhKirDexWpqoCA?key=lPrS3P9tXZMjc9bmE4-JEg)

Lets add a tag for this ASG. For the key enter Name and for the value enter **Dev-ASG-Server**. Scroll down and click **Next** then at the very bottom select **Create Auto Scaling Group**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdahMi7K5Mi-QGx46K5mdasxlBsvGPZUtAmMfh1G0LQBZI_JMr8NiwKla5iO9_UdIOpN2APSRXcwwPk_-LM8F_m9533FYuArpUnQosPqVQZktp2lefSXhFenHkZaiiH71IpeIWjNQ?key=lPrS3P9tXZMjc9bmE4-JEg)

We can now see the two EC2 instances that we created with the ASG.

