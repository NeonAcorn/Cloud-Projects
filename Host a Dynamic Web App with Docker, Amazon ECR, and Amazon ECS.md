
First I am going to set up my three-tier VPC

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-173.png?w=1021)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-174.png?w=1024)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-175.png?w=996)

For the route tables the **Public Route Table** has both public subnets associated to it while the **Private Route Table** has the private subnets associated to it based on the AZ they are in. The routes allow for **0.0.0.0/0** though the **Internet Gateway** I created for the **Public Route Table** and each **Private Route Tables** has **0.0.0.0/0** through a **NAT Gateway** in each AZ

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-177.png?w=1024)

These will be the security groups I will be creating with the following rules below

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-176.png?w=496)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-178.png?w=1011)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-179.png?w=1012)

We are going to be selecting the subnets for the Private Data Subnet so that would be **10.0.4.0/24** and **10.0.5.0/24**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-180.png?w=509)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-182.png?w=1009)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-183.png?w=699)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-184.png?w=1012)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-185.png?w=1010)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-186.png?w=1005)

These are the settings for the RDS database

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-187.png?w=1024)

I have created a ECR repo for a docker image I am going to push to it

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-188.png?w=1004)

I have also created a key pair to use for when I am going to ssh

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-189.png?w=946)

I am creating an instance named **Bastion Host** that will be in our **Dev-VPC** with the **Public Subnet AZ1**, the key pair I just created and the security group will be the **Bastion Security Group**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-190.png?w=1024)

I went ahead and created a **Target group** along with an **Application Load Balancer**. The Load balancers security group is the **ALB Security Group** we created and it is using both **Public Subnets** in both AZs. For the listeners I up the **HTTPS** listen to use my domain and certificate I registered in **Route53** then told the **HTTP** listen to redirect the full url over to the **HTTPS** one

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-191.png?w=1024)

```
{
    "Version": "2024-8-15",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::rentzone-app-env-variables/rentzone.env"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "s3:GetBucketLocation",
            "Resource": "arn:aws:s3:::rentzone-app-env-variables"
        }
    ]
}
```

I created an IAM role policy that looks like this

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-192.png?w=1024)

Next I went ahead and created an ECS cluster named **rentzone-cluster**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-193.png?w=699)

I created the Amazon ECS Task Definition. One important part is that when creating it I selected Amazon Linux x64_x86 since I am using windows if I was on a mac or linux I would have selected Amazon Linux ARM64

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-194.png?w=1008)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-195.png?w=1010)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-196.png?w=1012)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-197.png?w=1012)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-198.png?w=1019)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-199.png?w=1010)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-200.png?w=1024)