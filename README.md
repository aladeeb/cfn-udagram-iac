# Deploy a high-availability web app using CloudFormation (A Udacity project)

## Demo: 
- http://udagramalb-1107100627.us-east-1.elb.amazonaws.com/ (Down currently)

## Solution Design
![Solution Design Diagram](https://github.com/aladeeb/cfn-udagram-iac/blob/main/docs/Solution%20Design.png?raw=true)

## Server Requirements: 
- Hardware Specs: (2 vCPUs, 4GB RAM, 15GB storage).
- Software Specs: Ubuntu 18.4 and Apache server.
- 4 Servers should be deployed in service.
- Load balancer to distribute the load on the application servers. 

## Security Requirements: 
- IAM role to allow the servers to download files from specific S3 bucket.
- Server Ingress: HTTP allowed (LB will be accessing our servers).
- Server Egress: unrestricted internet access.
- LB Ingress: open to the internet on HTTP.
- LB Egress: only HTTP are allowed for the internal servers.
- Application to be deployed in the private subnets.
- LB to be deployed in the public subnet.

## Outputs: 
- LB DNS Name


## UserData script: 
```bash
    #!/bin/bash
    sudo apt-get update -y
    sudo apt-get install rpl -y
    sudo apt-get install unzip -y
    sudo apt-get install awscli -y
    sudo apt-get install apache2 -y
    sudo systemctl start apache2.service
    sudo chmod 2775 /var/www
    find /var/www -type d -exec sudo chmod 2775 {} \;
    find /var/www -type f -exec sudo chmod 0664 {} \;
    sudo rm /var/www/html/index.html
    sudo aws s3 cp s3://adeeb-udagram-app/src.zip /tmp
    sudo unzip -d /var/www/html /tmp/src.zip
    sudo rm /tmp/src.zip
    hostname=$(hostname -f)
    sudo rpl "EC2HOSTNAME" $hostname /var/www/html/index.html
```


## Run Locally

Follow the steps in the mentioned order and you have to wait till each stack completed on CloudFormation

Execute network.yaml

```bash
  aws cloudformation create-stack --template-body=file://templates/network.yaml --stack-name=udagram-network
```

Execute security-groups.yaml

```bash
  aws cloudformation create-stack --template-body=file://templates/security-groups.yaml --stack-name=udagram-security-groups
```

Execute alb.yaml

```bash
  aws cloudformation create-stack --template-body=file://templates/alb.yaml --stack-name=udagram-alb
```

Execute server.yaml

```bash
  aws cloudformation create-stack --template-body=file://templates/server.yaml --stack-name=udagram-server
```


## Author
- [@Ahmed Ayman Aladeeb' LinkedIn](https://www.linkedin.com/in/ahmedaymanaladeeb/)
- [@Ahmed Ayman Aladeeb' Github](https://github.com/aladeeb)
