# Deploy a high-availability web app using CloudFormation

## Server Requirements: 
- Launch configuration for the server that will be used by the ASG.
- 4 servers, two in each private subnet.
- Server Specs: (2 vCPUs, 4GB RAM, 15GB storage and based on Ubuntu18).

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

## Author
- [@Ahmed Ayman Aladeeb' LinkedIn](https://www.linkedin.com/in/ahmedaymanaladeeb/)
- [@Ahmed Ayman Aladeeb' Github](https://github.com/aladeeb)