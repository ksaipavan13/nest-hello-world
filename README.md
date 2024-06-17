# NestJs Hello World Application

## Introduction
This is a simple NestJs hello world application deployed on an AWS EC2 instance.

## Prerequisites
- AWS account with access to EC2
- SSH key pair for accessing EC2
- Basic knowledge of Node.js, Git, and SSH

## Installation Steps
1. Clone the repository:
   ```sh
   git clone https://github.com/ksaipavan13/nest-hello-world.git
   cd nest-hello-world
## Install dependencies
npm install
## Deployment Steps
Launch an EC2 instance and open ports 22 and 3000.
Connect to the EC2 instance using SSH.
ssh -i your-key-pair.pem ubuntu@13.233.183.73
Clone the repository on the EC2 instance:
git clone https://github.com/ksaipavan13/nest-hello-world.git
cd nest-hello-world
Install dependencies and start the application
npm install
npm run start
## Accessing the Application.

Open your web browser and navigate to http://13.233.183.73:3000.

## Additional Notes

Ensure that your security group settings are correctly configured.
Replace your-key-pair.pem and 13.233.183.73 with your actual key pair file and EC2 public DNS.
