Deploying a Static HTML Website on AWS EC2 Using Apache

Launch an EC2 instance from the AWS console.
Choose Ubuntu Server as the AMI, t2.micro as the instance type, and create or use an existing key pair.
In the security group, allow SSH on port 22 and HTTP on port 80.
Launch the instance and copy the public IP address.

Connect to the instance from VS Code terminal.
Use the SSH command:
ssh -i "your-key.pem" ubuntu@your-ec2-public-ip

Install Apache web server.
Run the following commands:
sudo apt update -y
sudo apt install apache2 -y
Start and enable Apache:
sudo systemctl start apache2
sudo systemctl enable apache2

Check Apache status.
Run sudo systemctl status apache2 to confirm it is active and running.

Test Apache in a browser.
Open http://your-ec2-public-ip
 in a browser.
You should see the default Apache Ubuntu page.

Clone your project from GitHub.
Install Git if necessary using sudo apt install git -y
Clone your repository:
cd /tmp
git clone https://github.com/ujwalshetty7022/my-project.git

Deploy the project.
Remove default Apache files and copy your project files:
sudo rm -rf /var/www/html/*
sudo cp -r /tmp/my-project/* /var/www/html/
Restart Apache:
sudo systemctl restart apache2

Allow web traffic.
Enable Apache through the firewall:
sudo ufw allow 'Apache Full'
sudo ufw enable
In AWS security group settings, ensure an inbound rule allows HTTP (port 80) from 0.0.0.0/0.

Access the website.
Go to http://your-ec2-public-ip
 in a browser.
Your HTML website should now be visible.

Update your website after changes.
When you make changes to your files and push them to GitHub, log into the EC2 instance and update with:
cd /tmp/my-project
git pull
sudo cp -r * /var/www/html/
sudo systemctl restart apache2

Summary:
Launch EC2 instance, connect through SSH, install Apache, clone the repository, copy files to the web directory, allow HTTP traffic, and access the site using the public IP.
To update, pull the latest changes from GitHub and copy them again to the Apache directory.
