
![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-201.png?w=1024)

I started off by creating an S3 bucket with versioning enabled

After we have that created I can going to work on creating the VPC using Terraform

```
#variables.tf

# VPC variables
variable "vpc_cidr" {
  default       = "10.0.0.0/16"
  description   = "vpc cidr block"
  type          = string
}

# -------------Subnets-------------
variable "public_subnet_az1_cidr" {
  default       = "10.0.0.0/24"
  description   = "public subnet az1 cidr block"
  type          = string
}

variable "public_subnet_az2_cidr" {
  default       = "10.0.1.0/24"
  description   = "public subnet az2 cidr block"
  type          = string
}

variable "private_app_subnet_az1_cidr" {
  default       = "10.0.2.0/24"
  description   = "private app subnet az1 cidr block"
  type          = string
}

variable "private_app_subnet_az2_cidr" {
  default       = "10.0.3.0/24"
  description   = "private app subnet az2 cidr block"
  type          = string
}


variable "private_data_subnet_az1_cidr" {
  default       = "10.0.4.0/24"
  description   = "private data subnet az1 cidr block"
  type          = string
}

variable "private_data_subnet_az2_cidr" {
  default       = "10.0.5.0/24"
  description   = "private data subnet az2 cidr block"
  type          = string
}

# -----------Availability Zones-----------
variable "availability_zone_us_east_1_a" {
  default     = "us-east-1a"
  description = "Availability Zone in the us-east-1 region"
  type        = string
}

variable "availability_zone_us_east_1_b" {
  default     = "us-east-1b"
  description = "Second Availability Zone in the us-east-1 region"
  type        = string
}

# --------Security Group Variables--------
variable "ssh_location" {
  default     = "0.0.0.0/0"
  description = "the ip address that we can ssh into the ec2 instance"
  type        = string
}

variable "internet_access" {
  default     = "0.0.0.0/0"
  description = "access to the open internet"
  type        = string
}

# ------------RDS Variables------------

variable "database_snapshot_identifier" {
  default     = "arn:aws:rds:us-east-1:851725572114:snapshot:database-1-snapshot"
  description = "rds snapshot arn"
  type        = string
}

variable "database_instance_class" {
  default     = "db.t3.micro"
  description = "the database instance type"
  type        = string
}

variable "database_instance_identifier" {
  default     = "database-1"
  description = "the database instance identifier"
  type        = string
}

variable "multi_az_deployment" {
  default     = "false"
  description = "create a standby db instance"
  type        = bool
}

# Application Load Balancer Variabes
variable "ssl_certificate_arn" {
  default     = "arn:aws:acm:us-east-1:851725572114:certificate/2f91c5e0-ae12-457b-8ff0-befe83f83d19"
  description = "ssl certificate arn"
  type        = string
}
```

I created a **variable.tf** file that will be used in the Terraform file for the VPC creation so that way if we ever want to make changes to specific things like the IP CIDR block for example it would be a lot quicker to just edit the variable file

```
#vpc.tf

# create vpc
# terraform aws create vpc
resource "aws_vpc" "vpc" {
  cidr_block              = var.vpc_cidr
  instance_tenancy        = "default"
  enable_dns_hostnames    = true

  tags      = {
    Name    = "dev vpc"
  }
}

# create internet gateway and attach it to vpc
# terraform aws create internet gateway
resource "aws_internet_gateway" "internet_gateway" {
  vpc_id    = aws_vpc.vpc.id

  tags      = {
    Name    = "dev internet gateway"
  }
}

# create public subnet az1
# terraform aws create subnet
resource "aws_subnet" "public_subnet_az1" {
  vpc_id                  = aws_vpc.vpc.id
  cidr_block              = var.public_subnet_az1_cidr
  availability_zone       = var.availability_zone_us_east_1_a
  map_public_ip_on_launch = true

  tags      = {
    Name    = "public subnet az1"
  }
}

# create public subnet az2
# terraform aws create subnet
resource "aws_subnet" "public_subnet_az2" {
  vpc_id                  = aws_vpc.vpc.id
  cidr_block              = var.public_subnet_az2_cidr
  availability_zone       = var.availability_zone_us_east_1_b
  map_public_ip_on_launch = true

  tags      = {
    Name    = "public subnet az2"
  }
}

# create route table and add public route
# terraform aws create route table
resource "aws_route_table" "public_route_table" {
  vpc_id       = aws_vpc.vpc.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.internet_gateway.id
  }

  tags       = {
    Name     = "public route table"
  }
}

# associate public subnet az1 to "public route table"
# terraform aws associate subnet with route table
resource "aws_route_table_association" "public_subnet_az1_route_table_association" {
  subnet_id           = aws_subnet.public_subnet_az1.id
  route_table_id      = aws_route_table.public_route_table.id
}

# associate public subnet az2 to "public route table"
# terraform aws associate subnet with route table
resource "aws_route_table_association" "public_subnet_2_route_table_association" {
  subnet_id           = aws_subnet.public_subnet_az2.id
  route_table_id      = aws_route_table.public_route_table.id
}

# create private app subnet az1
# terraform aws create subnet
resource "aws_subnet" "private_app_subnet_az1" {
  vpc_id                   = aws_vpc.vpc.id
  cidr_block               = var.private_app_subnet_az1_cidr
  availability_zone        = var.availability_zone_us_east_1_a
  map_public_ip_on_launch  = false

  tags      = {
    Name    = "private app subnet az1"
  }
}

# create private app subnet az2
# terraform aws create subnet
resource "aws_subnet" "private_app_subnet_az2" {
  vpc_id                   = aws_vpc.vpc.id
  cidr_block               = var.private_app_subnet_az2_cidr
  availability_zone        = var.availability_zone_us_east_1_b
  map_public_ip_on_launch  = false

  tags      = {
    Name    = "private app subnet az2"
  }
}

# create private data subnet az1
# terraform aws create subnet
resource "aws_subnet" "private_data_subnet_az1" {
  vpc_id                   = aws_vpc.vpc.id
  cidr_block               = var.private_data_subnet_az1_cidr
  availability_zone        = var.availability_zone_us_east_1_a
  map_public_ip_on_launch  = false

  tags      = {
    Name    = "private data subnet az1"
  }
}

# create private data subnet az2
# terraform aws create subnet
resource "aws_subnet" "private_data_subnet_az2" {
  vpc_id                   = aws_vpc.vpc.id
  cidr_block               = var.private_data_subnet_az2_cidr
  availability_zone        = var.availability_zone_us_east_1_b
  map_public_ip_on_launch  = false

  tags      = {
    Name    = "private data subnet az2"
  }
}
```

We can see in the terraform code it references the resource ID for a few things. Like **vpc_id** as an example. How we got that is by looking at this area of the code **resource “aws_vpc” “vpc” {** the **vpc_id** would just be taking this part **“aws_vpc” “vpc”** we just need to clean it up for the code, so it will become **aws_vpc.vpc.id**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-202.png?w=1024)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-203.png?w=1024)

We can see Terraform successfully created the VPC in AWS

```
# nat-gatway.tf

# allocate elastic ip. this eip will be used for the nat-gateway in the public subnet az1 
# terraform aws allocate elastic ip
resource "aws_eip" "eip_for_nat_gateway_az1" {
  domain    = "vpc" # update, this is the correct code.

  tags   = {
    Name = "nat gateway az1 eip"
  }
}

# allocate elastic ip. this eip will be used for the nat-gateway in the public subnet az2
# terraform aws allocate elastic ip
resource "aws_eip" "eip_for_nat_gateway_az2" {
  domain    = "vpc" # update, this is the correct code.

  tags   = {
    Name = "nat gateway az2 eip"
  }
}

# create nat gateway in public subnet az1
# terraform create aws nat gateway
resource "aws_nat_gateway" "nat_gateway_az1" {
  allocation_id = aws_eip.eip_for_nat_gateway_az1.id
  subnet_id     = aws_subnet.public_subnet_az1.id

  tags   = {
    Name = "nat gateway az1"
  }

  # to ensure proper ordering, it is recommended to add an explicit dependency
  # on the internet gateway for the vpc.
  depends_on = [aws_internet_gateway.internet_gateway]
}

# create nat gateway in public subnet az2
# terraform create aws nat gateway
resource "aws_nat_gateway" "nat_gateway_az2" {
  allocation_id = aws_eip.eip_for_nat_gateway_az2.id
  subnet_id     = aws_subnet.public_subnet_az2.id

  tags   = {
    Name = "nat gateway az2"
  }

  # to ensure proper ordering, it is recommended to add an explicit dependency
  # on the internet gateway for the vpc.
  depends_on = [aws_internet_gateway.internet_gateway]
}

# create private route table az1 and add route through nat gateway az1
# terraform aws create route table
resource "aws_route_table" "private_route_table_az1" {
  vpc_id            = aws_vpc.vpc.id

  route {
    cidr_block      = "0.0.0.0/0"
    nat_gateway_id  = aws_nat_gateway.nat_gateway_az1.id
  }

  tags   = {
    Name = "private route table az1"
  }
}

# associate private app subnet az1 with private route table az1
# terraform aws associate subnet with route table
resource "aws_route_table_association" "private_app_subnet_az1_route_table_az1_association" {
  subnet_id         = aws_subnet.private_app_subnet_az1.id
  route_table_id    = aws_route_table.private_route_table_az1.id
}

# associate private data subnet az1 with private route table az1
# terraform aws associate subnet with route table
resource "aws_route_table_association" "private_data_subnet_az1_route_table_az1_association" {
  subnet_id         = aws_subnet.private_data_subnet_az1.id
  route_table_id    = aws_route_table.private_route_table_az1.id
}

# create private route table az2 and add route through nat gateway az2
# terraform aws create route table
resource "aws_route_table" "private_route_table_az2" {
  vpc_id            = aws_vpc.vpc.id

  route {
    cidr_block      = "0.0.0.0/0"
    nat_gateway_id  = aws_nat_gateway.nat_gateway_az2.id
  }

  tags   = {
    Name = "private route table az2"
  }
}

# associate private app subnet az2 with private route table az2
# terraform aws associate subnet with route table
resource "aws_route_table_association" "private_app_subnet_az2_route_table_az2_association" {
  subnet_id         = aws_subnet.private_app_subnet_az2.id
  route_table_id    = aws_route_table.private_route_table_az2.id
}

# associate private data subnet az2 with private route table az2
# terraform aws associate subnet with route table
resource "aws_route_table_association" "private_data_subnet_az2_route_table_az2_association" {
  subnet_id         = aws_subnet.private_data_subnet_az2.id
  route_table_id    = aws_route_table.private_route_table_az2.id
}
```

Here is the code we are using to create the NAT gateway

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-204.png?w=934)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-205.png?w=949)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-206.png?w=918)

We can see Terraform successfully created the specified resources

```
# security-group.tf

# create security group for the application load balancer
# terraform aws create security group
resource "aws_security_group" "alb_security_group" {
  name        = "alb security group"
  description = "enable http/https access on port 80/443"
  vpc_id      = aws_vpc.vpc.id

  ingress {
    description      = "http access"
    from_port        = 80
    to_port          = 80
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
  }

  ingress {
    description      = "https access"
    from_port        = 443
    to_port          = 443
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
  }

  egress {
    from_port        = 0
    to_port          = 0
    protocol         = -1
    cidr_blocks      = ["0.0.0.0/0"]
  }

  tags   = {
    Name = "alb security group"
  }
}

# create security group for the bastion host aka jump box
# terraform aws create security group
resource "aws_security_group" "ssh_security_group" {
  name        = "ssh security group"
  description = "enable ssh access on port 22"
  vpc_id      = aws_vpc.vpc.id

  ingress {
    description      = "ssh access"
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    cidr_blocks      = [var.ssh_location]
  }

  egress {
    from_port        = 0
    to_port          = 0
    protocol         = -1
    cidr_blocks      = [var.internet_access]
  }

  tags   = {
    Name = "ssh security group"
  }
}

# create security group for the web server
# terraform aws create security group
resource "aws_security_group" "webserver_security_group" {
  name        = "webserver security group"
  description = "enable http/https access on port 80/443 via alb sg and access on port 22 via ssh sg"
  vpc_id      = aws_vpc.vpc.id

  ingress {
    description      = "http access"
    from_port        = 80
    to_port          = 80
    protocol         = "tcp"
    security_groups  = [aws_security_group.alb_security_group.id]
  }

  ingress {
    description      = "https access"
    from_port        = 443
    to_port          = 443
    protocol         = "tcp"
    security_groups  = [aws_security_group.alb_security_group.id]
  }

  ingress {
    description      = "ssh access"
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    security_groups  = [aws_security_group.ssh_security_group.id]
  }

  egress {
    from_port        = 0
    to_port          = 0
    protocol         = -1
    cidr_blocks      = [var.internet_access]
  }

  tags   = {
    Name = "webserver security group"
  }
}

# create security group for the database
# terraform aws create security group
resource "aws_security_group" "database_security_group" {
  name        = "database security group"
  description = "enable mysql/aurora access on port 3306"
  vpc_id      = aws_vpc.vpc.id

  ingress {
    description      = "mysql/aurora access"
    from_port        = 3306
    to_port          = 3306
    protocol         = "tcp"
    security_groups  = [aws_security_group.webserver_security_group.id]
  }

  egress {
    from_port        = 0
    to_port          = 0
    protocol         = -1
    cidr_blocks      = [var.internet_access]
  }

  tags   = {
    Name = "database security group"
  }
}
```

One thing to note when making the SSH Security Group I put for the IP **0.0.0.0/0** never do this. You want to limit it to your **IP**. I am just using 0.0.0.0/0 since this is for testing and going to be deleted after I put this project together

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-207.png?w=942)

```
# rds.tf

# create database subnet group
# terraform aws db subnet group
resource "aws_db_subnet_group" "database_subnet_group" {
  name         = "db subents"
  subnet_ids   = [aws_subnet.private_data_subnet_az1.id, aws_subnet.private_data_subnet_az2.id]
  description  = "subnets for database instance"

  tags   = {
    Name = "database subnets"
  }
}

# get the latest db snapshot
# terraform aws data db snapshot
data "aws_db_snapshot" "latest_db_snapshot" {
  db_snapshot_identifier = var.database_snapshot_identifier
  most_recent            = true
  snapshot_type          = "manual"
}

# create database instance restored from db snapshots
# terraform aws db instance
resource "aws_db_instance" "database_instance" {
  instance_class          = var.database_instance_class
  skip_final_snapshot     = true
  availability_zone       = var.availability_zone_us_east_1_b
  identifier              = var.database_instance_identifier
  snapshot_identifier     = data.aws_db_snapshot.latest_db_snapshot.id
  db_subnet_group_name    = aws_db_subnet_group.database_subnet_group.name
  multi_az                = var.multi_az_deployment
  vpc_security_group_ids  = [aws_db_subnet_group.database_subnet_group.id]
}
```

We are going to create an RDS database with an existing RDS snapshot

```
# alb.tf

# create application load balancer
# terraform aws create application load balancer
resource "aws_lb" "application_load_balancer" {
  name               = "dev-alb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.alb_security_group.id]

  subnet_mapping {
    subnet_id = aws_subnet.public_subnet_az1.id
  }

  subnet_mapping {
    subnet_id = aws_subnet.public_subnet_az2.id
  }

  enable_deletion_protection = false

  tags   = {
    Name = "dev alb"
  }
}

# create target group
# terraform aws create target group
resource "aws_lb_target_group" "alb_target_group" {
  name        = "Dev-TG"
  target_type = "instance"
  port        = 80
  protocol    = "HTTP"
  vpc_id      = aws_vpc.vpc.id

  health_check {
    healthy_threshold   = 5
    interval            = 30
    matcher             = "200,301,302"
    path                = "/"
    port                = "traffic-port"
    protocol            = "HTTP"
    timeout             = 5
    unhealthy_threshold = 2
  }
}

# create a listener on port 80 with redirect action
# terraform aws create listener
resource "aws_lb_listener" "alb_http_listener" {
  load_balancer_arn = aws_lb.application_load_balancer.arn
  port              = 80
  protocol          = "HTTP"

  default_action {
    type = "redirect"

    redirect {
      host        = "#{host}"
      path        = "/#{path}"
      port        = 443
      protocol    = "HTTPS"
      status_code = "HTTP_301"
    }
  }
}

# create a listener on port 443 with forward action
# terraform aws create listener
resource "aws_lb_listener" "alb_https_listener" {
  load_balancer_arn  = aws_lb.application_load_balancer.arn
  port               = 443
  protocol           = "HTTPS"
  ssl_policy         = "ELBSecurityPolicy-2016-08"
  certificate_arn    = var.ssl_certificate_arn

  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.alb_target_group.arn
  }
}
```

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-208.png?w=1024)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-209.png?w=1024)

We have created the Application Load Balancer and the Target Group

```
# sns.tf

# create an sns topic
# terraform aws create sns topic
resource "aws_sns_topic" "user_updates" {
  name      = "dev-sns-topic"
}

# create an sns topic subscription
# terraform aws sns topic subscription
resource "aws_sns_topic_subscription" "notification_topic" {
  topic_arn = aws_sns_topic.user_updates.arn
  protocol  = "email"
  endpoint  = var.operator_email
}
```

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-210.png?w=1024)

Terraform has successfully created the SNS topic

```
# asg.tf

# create a launch template
# terraform aws launch template
resource "aws_launch_template" "webserver_launch_template" {
  name          = var.lunch_template_name
  image_id      = var.ec2_image_id
  instance_type = var.ec2_instance_type
  key_name      = var.ec2_key_pair
  description   = "launch template for asg"

  monitoring {
    enabled = false
  }

  vpc_security_group_ids = [aws_security_group.webserver_security_group.id]
}

# create auto scaling group
# terraform aws autoscaling group
resource "aws_autoscaling_group" "auto_scaling_group" {
  vpc_zone_identifier = [aws_subnet.private_app_subnet_az1.id, aws_subnet.private_app_subnet_az2.id]
  desired_capacity    = 2
  max_size            = 4
  min_size            = 1
  name                = "dev-asg"
  health_check_type   = "ELB"

  launch_template {
    name    = aws_launch_template.webserver_launch_template.name
    version = "$Latest"
  }

  tag {
    key                 = "Name"
    value               = "asg-webserver"
    propagate_at_launch = true
  }

  lifecycle {
    ignore_changes      = [target_group_arns]
  }
}

# attach auto scaling group to alb target group
# terraform aws autoscaling attachment
resource "aws_autoscaling_attachment" "asg_alb_target_group_attachment" {
  autoscaling_group_name = aws_autoscaling_group.auto_scaling_group.id
  lb_target_group_arn    = aws_lb_target_group.alb_target_group.arn
}

# create an auto scaling group notification
# terraform aws autoscaling notification
resource "aws_autoscaling_notification" "webserver_asg_notifications" {
  group_names = [aws_autoscaling_group.auto_scaling_group.name]

  notifications = [
    "autoscaling:EC2_INSTANCE_LAUNCH",
    "autoscaling:EC2_INSTANCE_TERMINATE",
    "autoscaling:EC2_INSTANCE_LAUNCH_ERROR",
    "autoscaling:EC2_INSTANCE_TERMINATE_ERROR",
  ]

  topic_arn = aws_sns_topic.user_updates.arn
}
```

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-211.png?w=866)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-212.png?w=1024)

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-213.png?w=922)

We can see the instances have been registered with the target group and that they are also currently running as well.

```
# route-53.tf

# get hosted zone details
# terraform aws data hosted zone
data "aws_route53_zone" "hosted_zone" {
  name = var.domain_name
}

# create a record set in route 53
# terraform aws route 53 record
resource "aws_route53_record" "site_domain" {
  zone_id = data.aws_route53_zone.hosted_zone.zone_id
  name    = var.record_name
  type    = "A"

  alias {
    name                   = aws_lb.application_load_balancer.dns_name
    zone_id                = aws_lb.application_load_balancer.zone_id
    evaluate_target_health = true
  }
}
```

Terraform will use this to create the record set in **Route 53**

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-214.png?w=1024)

We can see the website is up and running