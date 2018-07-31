# Amazon Lightsail Server

<!-- Instructions taken from the following: -->
https://github.com/callforsky/udacity-linux-configuration

### Part I
1. Sign up for an Amazon Lightsail account. 
	https://aws.amazon.com/lightsail/
2. Create an Instance
3. Select a Linux/Unix platform
4. Select an OS Only blueprint with Ubuntu 16.04 LTS
5. Name your Instance
6. Click "create"
7. Wait 5-10 minutes for the Instance to be created.

### Part II
1. Write down the public IP address for your Instance.
2. Click on Accounts Page
3. Click on SSH Keys
4. Download SSH Key for server Instance.
5. Place .pem file in root .ssh hidden files.
6. Change .pem file name to udacity-item-catalog.pem

## Firewall setup
1. In terminal, type the following:
	chmod 600 ~/.ssh/udacity-item-catalog.pem
2. In terminal, type the following:
	sudo ssh -i ~/.ssh/udacity-item-catalog.pem ubuntu@[Your PUBLIC IP ADDRESS] 
3. 


