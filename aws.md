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
1. Under Firewall tab, click "+ Add Another" and click Protocol TCP Port Range 123. 
2. Repeat the above steps, but add TCP Port Range 2200.
3. Click "save".

## Server Configuration
1. In terminal, type the following:
	chmod 600 ~/.ssh/udacity-item-catalog.pem
2. In terminal, type the following:
	sudo ssh -i ~/.ssh/udacity-item-catalog.pem ubuntu@[Your PUBLIC IP ADDRESS] 
3. Amazon Lightsail does not allow you to login as the root user. Therefore,
we switch to become the root user by typing the following:
	sudo su -
4. In terminal, type sudo adduser grader
5. Create a new file under sudoers.directory with the following command:
	sudo nano /etc/sudoers.d/grader
6. Fill the file with the following:
	grader ALL=(ALL:ALL) ALL
7. Save the file.

x. Run the following commands:
	$ sudo apt-get update
	$ sudo apt-get upgrade
	$ sudo apt-get install finger

8. Open a new Terminal window (Command+N) and input 
	$ ssh-keygen -f ~/.ssh/udacity_key.rsa
9. Stay on the same Terminal window, input 
	$ cat ~/.ssh/udacity_key.rsa.pub 
to read the public key. 
10. Open a NEW TERMINAL WINDOW and input the following:
	$ ssh-keygen -f ~/.ssh/udacity_key.rsa
	$ cat ~/.ssh/udacity_key.rsa.pub
11. Copy the public key
12. Go back to the old (first) terminal window. 
	$ cd /home/grader
	$ mkdir .ssh
	$ touch .ssh/authorized_keys
	$ nano .ssh/authorized_keys
xx. Change the permission: 
	$ sudo chmod 700 /home/grader/.ssh
	$ sudo chmod 644 /home/grader/.ssh/authorized_keys
xx. Change the owner from root to grader:
	$ sudo chown -R grader:grader /home/grader/.ssh



Lets enforce key authentication from the ssh configuration file by editing $ sudo nano /etc/ssh/sshd_config. Find the line that says PasswordAuthentication and change it to no. Also find the line that says Port 22 and change it to Port 2200. Lastly change PermitRootLogin to no.



Deploying Catalog Application:

ssh -i ~/.ssh/udacity_key.rsa -p 2200 grader@13.114.136.213
