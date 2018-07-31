# Linux Security Notes

Distro Watch has been around since 2001 as a source of news about Linux
distributions and other Free and Open-Source operating systems.
(My personal favorite is the Page Hit Rankings section on the homepage)
	https://distrowatch.com/

Wikipedia article comparing current and former Linux distributions.
	https://en.wikipedia.org/wiki/Comparison_of_Linux_distributions

A full path is also referred to as an absolute path.
	-al
	http://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html

How to add/edit/save a directory to path
	echo $PATH
	https://askubuntu.com/questions/60218/how-to-add-a-directory-to-the-path


# Linux Security 

ls -al /home/ubuntu/.ssh
Permission denied

sudo ls -al /home/ubuntu/.ssh


## sudo vs su
The su command stands for super user or root user. ... Comparing the both,
sudo lets one use the user account password to run system command. On the
other hand, su forces one to share the root passwords to other users.
Also, sudo doesn't activate the root shell and runs a single command.

It's typically recommended that you not use the su command when you can
use sudo for one/few operations instead. 

## Package Source List in Linux
cat /etc/apt/sources.list


<!-- Testing on June 15, 2018 -->
## Updating Available Package Lists
	[x] updating package source list
			sudo apt-get update

## Upgrading Installed Packages
	[x] updating software
			sudo apt-get upgrade

## Other package related tasks
	man apt-get
	<!-- remove packages no longer required -->
	[x] sudo apt-get autoremove
	<!-- install an application called finger -->
	[x] sudo apt-get install finger

## Discovering Packages
	https://packages.ubuntu.com/
	<!-- trusty package list -->
	https://packages.ubuntu.com/trusty/

## Using Finger
	<!-- Output all of the users currently logged into the system -->
	[x] finger
	<!-- Additional information about a specific user -->
		sudo apt install vagrant
	[x] finger vagrant
	[x] (vagrant, no such user)
<!-- End Testing June 15, 2018 10:57AM -->

## etc/passwd
	[x] cat /etc/passwd
	<!-- each line is organized in this format: -->
	username:password:UID:GID:UID info:home directory:command/shell
	1. username: the user’s login name
	2. password: the password, will simply be an x if it’s encrypted
	3. user ID (UID): the user’s ID number in the system. 0 is root, 1-99 are for predefined users, and 100-999 are for other system accounts
	4. group ID (GID): Primary group ID, stored in /etc/group.
	5. user ID info: Metadata about the user; phone, email, name, etc.
	6. home directory: Where the user is sent upon login. Generally /home/
	7. command/shell: The absolute path of a command or shell (usually /bin/bash). Does not have to be a shell though!

## User Management & Creating a New User
	sudo adduser student
		student
		Kenneth P Chang
		pw: laziness
		room #: 27
	<!-- add password (I just used student as the pw for this project -->
	confirm adduser by using the finger command:
	[x] finger student

## Connecting as the New User
	New Terminal --> vagrant up
	ssh student@127.0.0.1 -p 2222
	yes
	password

## etc/sudoers
	sudo cat /etc/passwd
	enter password
	student is not in the sudoers file.  This incident will be reported.
	<!-- our student does not have permission to use the sudo command -->
	<!-- switch back to vagrant user (because vagrant/root user can run admin tasks) -->
	list of users that are allowed to run admin tasks are in the /etc/sudoers file
	sudo cat /etc/sudoers
	note include directives: #includedir /etc/sudoers.d keeps our user information safe from being deleted.
	sudo ls /etc/sudoers.d

## Giving sudo Access
	Ubuntu Documentation: Sudoers
	https://help.ubuntu.com/community/Sudoers
	<!-- Giving student access to sudo themselves -->
	sudo cp /etc/sudoers.d/vagrant /etc/sudoers.d/student
	<!-- cp is copy, nano is edit -->
	sudo nano /etc/sudoers.d/student 
	<!-- change vagrant to student example below -->
	# CLOUD_IMG: This file was created/modified by the Cloud Image build process
	vagrant ALL=(ALL) NOPASSWD:ALL
	<!-- change to the following: -->
	# CLOUD_IMG: This file was created/modified by the Cloud Image build process
	student ALL=(ALL) NOPASSWD:ALL
	<!-- Save the file -->
	<!-- Log back into student (after exit) with the following command: -->
	exit
	ssh student@127.0.0.1 -p 2222
	sudo cat /etc/passwd 
	<!-- (student user now has access to sudo) -->

## Resetting Passwords
	Forcing student user to reset their password:
	Using the passwd command, use the following:
	-e sets the password to expire
	
	sudo passwd -e student

	new passwd = laziness
	
## Generating Key Pairs
	Generate key pairs locally. 
	ssh-keygen
	You will be asked to give a file name for the keygen
	file name will be linuxCourse
	/Users/everydaykenneth/.ssh/linuxCourse
	enter passphrase twice:
	passphrase = laziness
	Linux has generated two files: 
	[] Your identification has been saved in /Users/everydaykenneth/.ssh/linuxCourse.
	[] Your public key has been saved in /Users/everydaykenneth/.ssh/linuxCourse.pub.
	.pub file is what we will place on our server to enable authentication
	SSH version 2 protocol supports DSA, ECDSA, ED25519 and RSA key types.
	You can find this information by running man ssh-keygen, then -t
	MDA and SHA256 are hashing algorithms not suitable for public key encryption.

## Manually Installing a Public Key
	First, log into the server as the student. (vagrant up, ssh student@127.0.0.1 -p 2222)
	Now create a directory called .ssh with (mkdir .ssh)
	Create a new file called authorized_keys (touch .ssh/authorized_keys)
	Now go back into the local machine (cd ~) and type (cat .ssh/linuxCourse.pub)
	Copy and paste the ssh-rsa key from above - the last line should be .local
	Back on the server as the student user (vagrant up, ssh student@127.0.0.1 -p 2222), edit the authorized key file with the following terminal command:
	nano .ssh/authorized_keys
	(paste the cat .ssh/linuxCourse.pub content into the file.)

	Set up specific permissions on the ssh directory and the authorized_keys file:
	In terminal, run (ch mod 700 .ssh) in the student file login directory
	In terminal, run (ch mod 644 .ssh/authorized_keys) in the student file login authorized_keys file

	Now that we're all done, go back out to the root project folder. Instead of logging in with (ssh student@127.0.0.1 -p 2222), we can use the following:
	ssh student@127.0.0.1 -p 2222 -i ~/.ssh/linuxCourse

## Forcing Key Based Authentication
	Disable password based logins. This will force users to login only using the keypair. To do this, you have to edit the configuration file for sshd.

	login as student:
	sudo nano /etc/ssh/sshd_config
	change PasswordAuthentication to no, then save file. Press enter.
	sudo service ssh restart
	Now all users will be forced to login using a keypair. SSH will not allow users to login w/a username and passwd anymore.


## File Permissions
	check filePermissions.png to compare with ls -al in student terminal

	octal permissions: 0 = no permissions whatsoever. 
	see octalPermissions.png file for summary.

	example:
	-rw-r--r-- 1 student student 3637 Apr 24 07:45 .bashrc
	 6  4  4	= 644

	read = 4, write = 2, x(execute) = 1, 0 = no permissions.

## Change Group and Change Owner commands
	sudo chgrp [user] .[filename]
	sudo chown [user] .[filename]

## Firewalls and UFW (still logged into student ssh)
	Check defaultPorts.png picture for common default ports.
	
	sudo ufw status
	Status: inactive
	
	sudo ufw default deny incoming
	Default incoming policy changed to 'deny'
	(be sure to update your rules accordingly)
	
	sudo ufw default allow outgoing
	Default outgoing policy changed to 'allow'
	(be sure to update your rules accordingly)

	Check firewall:
	sudo ufw status 
	Status: inactive
	We must manually turn on the firewall ourselves after configuring the firewall.

## Configuring ports in UFW
	sudo ufw allow ssh
	Rules updated
	Rules updated (v6)
	sudo ufw allow 2222/tcp (because vagrant virtual machine is port 2222)
	Rules updated
	Rules updated (v6)
	sudo ufw allow www (basic http server)
	Rules updated
	Rules updated (v6)
	sudo ufw enable
	Firewall is active and enabled on system startup
	sudo ufw status

	Status: active

	To                         Action      From
	--                         ------      ----
	22                         ALLOW       Anywhere
	2222/tcp                   ALLOW       Anywhere
	80/tcp                     ALLOW       Anywhere
	22 (v6)                    ALLOW       Anywhere (v6)
	2222/tcp (v6)              ALLOW       Anywhere (v6)
	80/tcp (v6)                ALLOW       Anywhere (v6)


## Instructions on how to configure your servers
	"LAMP" Stack (Linux, Apache, MySQL, PHP)
	https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-14-04
	
	"LEMP" Stack (Linux, nginx, MySQL, PHP)
	https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-14-04
	
	PEPS Mail and File Storage
	https://www.digitalocean.com/community/tutorials/how-to-run-your-own-mail-server-and-file-storage-with-peps-on-ubuntu-14-04
	
	Mail-in-a-Box Email Server
	https://www.digitalocean.com/community/tutorials/how-to-run-your-own-mail-server-with-mail-in-a-box-on-ubuntu-14-04
	
	Lita IRC Chat Bot
	https://www.digitalocean.com/community/tutorials/how-to-install-the-lita-chat-bot-for-irc-on-ubuntu-14-04






