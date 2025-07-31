
```
aws ecr create-repository --repository-name jupiter --region us-east-1
```

I am creating a ECR through the command prompt with this command

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-148.png?w=1024)

We can see the Repository has been created.

```
docker tag jupiter 851725572114.dkr.ecr.us-east-1.amazonaws.com/jupit
```

I have a docker image that I am now going to tag with the AWS ECR URI

```
aws ecr get-login-password | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
```

Once tagged I am now making sure to log into the Amazon ECR

```
aws ecr get-login-password | docker login --username AWS --password-stdin 851725572114.dkr.ecr.us-east-1.amazonaws.com
```

Now that I have logged in I am using this command to push the docker image over to the AWS ECR

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-150.png?w=1024)

If we head over to ECR we can see the image is now in the repository

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-149.png?w=1024)

Now I am going to be creating a three-tier VPC

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-152.png?w=894)

I am creating a VPC with the name **Dev-VPC**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-153.png?w=1024)

I am creating 6 subnets, 2 public ones and 4 private ones. You can see the IPs I will be using for them

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-154.png?w=990)

Next a Internet gateway will be created

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-151.png?w=972)

I am going to create 2 NAT gateways for AZ1 use the public subnet AZ1 and for AZ2 use Public subnet AZ2

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-155.png?w=831)

I will also created 3 route tables. For the public one associate the 2 public subnets to it. Then edit the route and add 0.0.0.0/0 to the internet gateway. For the private route tables associate the private subnets that reflect the correct AZ to each one. For the routes add 0.0.0.0/0 to the NAT gateway that reflects the correct AZ.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-156.png?w=1024)

I am going to create a security group that contains the following

For the next security group we will name it **Container Security Group** it will use the same **HTTP** and **HTTPS** but instead of using **0.0.0.0/0** it will go to the security group we just created **ALB Security Group**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-157.png?w=1024)

Now we are going to create a target group. I am going to name it **Dev-TG**

all the settings will be left a default

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-158.png?w=1024)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-159.png?w=1024)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-160.png?w=1024)

Now I am going to make an Application Load Balancer

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-161.png?w=1012)

Now I am making an ECS cluster that will be using the AWS Fargate service so we do not need to worry about the underlying infrastructure

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-162.png?w=1024)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-163.png?w=1024)

If you build the docker container on a windows select **Linux/X86_64** if it was on a mac select **Linux/ARM64**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-164.png?w=1024)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-165.png?w=1024)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-166.png?w=1024)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-167.png?w=1024)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-169.png?w=1024)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-171.png?w=446)

I went ahead and created an A record for the load balancer

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-172.png?w=741)

I also updated the listener on the load balancer to redirect traffic to **HTTP** over to **HTTPS**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-170.png?w=1024)

Now we can see the website is up and running in a highly available configuration