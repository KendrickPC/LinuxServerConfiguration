# Amazon Lightsail Setup

1. Sign up for a Amazon Web Services Lightsail Account
https://aws.amazon.com/
2. Sign into AWS Console
![Create Instance in AWS](readme_images/Create_Instance.png)
3. Private IP = 172.26.1.41

   Public IP = 52.194.229.119
   
   Host Name = ec2-52-194-229-119.ap-northeast-1.compute.amazonaws.com

   Get the hostname from http://www.hcidata.info/host2ip.cgi

4. Download SSH Keypairs
![Accounts Page](readme_images/accounts_page.png)
5. Add TCP Ports
![Firewall](readme_images/firewall.png)
6. Now we are finished with the Amazon Lightsail Setup! 

# Linux Server Configuration

1. Show all files in Mac OSX by typing `$ killall Finder` in the terminal. 
2. Input in terminal `$ defaults write com.apple.finder AppleShowAllFiles TRUE`. 
3. Locate the hidden .ssh folder in the root of your Mac.
4. Move the downloaded `.pem` file into your `.ssh` folder.
![.pem key to .ssh folder](readme_images/ssh.png)
5. Rename the `.pem` file to `udacity.pem`
6. To make the public key usable and secure, go back to your terminal and input `$ chmod 600 ~/.ssh/udacity.pem`
7. Log into Amazon Lightsail Server with `$ ssh -i ~/.ssh/udacity.pem ubuntu@52.194.229.119`
8. Run `$ sudo apt-get update`
9. Run `$ sudo apt-get upgrade`
10. Run `$ sudo apt-get install finger`
11. Type `$ sudo su -` to become a root user. 
12. Type  `$ sudo adduser grader` to create a user 'grader' 
13. Enter `grader` UNIX password as `udacity`
14. Create a new file under the sudoers directory: `$ sudo nano /etc/sudoers.d/grader`.
15. Fill that file with `grader ALL=(ALL:ALL) ALL` using nano and save it.
![sudoers](readme_images/sudoers.png)
16. In order to prevent the `$ sudo: unable to resolve host error`, edit the hosts by `$ sudo nano /etc/hosts`, and then add `127.0.1.1 ip-10-20-37-65` under `127.0.1.1:localhost`

Resource Links:
https://github.com/callforsky/udacity-linux-configuration