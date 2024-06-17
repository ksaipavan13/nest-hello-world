# Deploying NestJS HelloWorld Application on EC2 with CI/CD using GitHub Actions

## Introduction

This guide walks you through deploying a NestJS HelloWorld sample application on an AWS EC2 instance and implementing a CI/CD pipeline using GitHub Actions.

## Prerequisites

- AWS account with access to EC2
- SSH key pair for accessing EC2
- Basic knowledge of Node.js, Git, and SSH

## Manual Deployment Steps

### 1. Launch and Connect to an EC2 Instance

1. **Launch an EC2 instance:**
   - Choose an Amazon Linux 2 or Ubuntu 18.04 AMI.
   - Select an instance type (e.g., t2.micro).
   - Configure security group to allow SSH (port 22) and HTTP (port 3000) access.

2. **Connect to the EC2 instance:**
   ```bash
   ssh -i /path/to/your-key.pem ubuntu@your-ec2-public-ip
2. Install Necessary Software
Update and install Node.js and npm:

bash
Copy code
sudo apt update && sudo apt upgrade -y
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt install -y nodejs
Install pm2 to manage the application process:

bash
Copy code
sudo npm install pm2 -g
3. Clone the Repository and Install Dependencies
Clone the repository:

bash
Copy code
git clone https://github.com/ksaipavan13/nest-hello-world.git
cd nest-hello-world
Install dependencies:

bash
Copy code
npm install
4. Run the Application Using pm2
Start the application:

bash
Copy code
pm2 start dist/main.js --name nestjs-app
Set pm2 to start on boot:

bash
Copy code
pm2 startup systemd
sudo env PATH=$PATH:/usr/bin pm2 startup systemd -u ubuntu --hp /home/ubuntu
pm2 save
5. Ensure Application is Running
Check pm2 status:

bash
Copy code
pm2 status
Access the application:

Open your browser and navigate to http://your-ec2-public-ip:3000.
Implement CI/CD Using GitHub Actions

1. Set Up GitHub Actions Workflow
Create a GitHub Actions workflow:

Create a .github/workflows/deploy.yml file in your repository.
Add the following configuration:

yaml
Copy code
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'

      - name: Install Dependencies
        run: npm install

      - name: Run Tests
        run: npm test

      - name: Deploy to EC2
        env:
          EC2_HOST: ${{ secrets.EC2_HOST }}
          EC2_USER: ${{ secrets.EC2_USER }}
          EC2_KEY: ${{ secrets.EC2_KEY }}
        run: |
          echo "$EC2_KEY" > key.pem
          chmod 400 key.pem
          ssh -o StrictHostKeyChecking=no -i key.pem $EC2_USER@$EC2_HOST <<EOF
          cd /home/ubuntu/nest-hello-world
          git pull origin main
          npm install
          npm run build
          pm2 restart nestjs-app
          EOF
2. Add GitHub Secrets
Go to Your Repository Settings:

Navigate to your repository on GitHub.
Click on "Settings" > "Secrets and variables" > "Actions."
Add the Following Secrets:

EC2_HOST: Your EC2 instance's public IP address.
EC2_USER: The username to SSH into your EC2 instance (usually ubuntu).
EC2_KEY: The private SSH key used to access your EC2 instance. Copy the contents of your .pem file and paste it here.
3. Trigger CI/CD Pipeline
Commit and Push Changes:

bash
Copy code
git add .
git commit -m "Setup CI/CD pipeline"
git push origin main
Observe GitHub Actions:

Go to the "Actions" tab in your GitHub repository.
Ensure the workflow runs successfully and deploys your application to the EC2 instance.
Final Verification

Verify Application Status:

Check pm2 status on the EC2 instance to ensure the application is running.
Verify Application Accessibility:

Access the application in the browser using the public IP of the EC2 instance: http://your-ec2-public-ip:3000.
Conclusion

Following these steps, you should successfully deploy the NestJS HelloWorld application on an EC2 instance and set up a CI/CD pipeline using GitHub Actions.

sql
Copy code

4. **Commit the Changes:**
   - Scroll to the bottom of the page.
   - Add a commit message (e.g., "Add deployment instructions").
   - Click the "Commit new file" or "Commit changes" button.

### Your README.md file should now be updated with the detailed instructions on how to deploy the application manually and set up CI/CD using GitHub Actions. Let me know if you need any more assistance!





