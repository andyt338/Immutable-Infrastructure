# Description

This project creates a Hello World website on an nginx container on an AWS instance.  The instance hosting the container (and therefore the container itself) can then be swapped out on the load balancer with zero downtime.  So with each deployment, you're actually tearing down the instance and container and creating new ones.  This allows for immutable infrastructure where changes to the website are not done in-place - they are done by creating an entirely new container and server.  This has many advantages, incluing avoiding configuration drift and having snowflake servers.  

# Prerequisites

Terraform

AWS Account + API keys

# How to Run

First edit terraform.tfvars.example and change the name to terraform.tfvars.  Also, add the .pem file used for AWS instances to the root of this directory.  This is the key_name variable.  It doesn't have '.pem' in the terraform.tfvars file.  To get the website up, run the following:

```
terraform init
terraform apply
```

Now check the AWS console for the instance that was created.  Browse its public IP to see your website.  It should say 'Hello World'.  To test the zero-downtime instance-swapping, change index.html, and run the following:

```
terraform taint aws_instance.web
terraform apply
```

Now go back to your browser and refresh the page multiple times until the new index.html page is displayed.  It should take about 3 minutes, give or take, to create the new instance and destroy the old one.

Destroy what you've created:

```
terraform destroy
```
