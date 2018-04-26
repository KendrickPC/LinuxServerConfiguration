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
The su command stands for super user or root user. ... Comparing the both, sudo lets one use the user account password to run system command. On the other hand, su forces one to share the root passwords to other users. Also, sudo doesn't activate the root shell and runs a single command.

It's typically recommended that you not use the su command when you can use sudo for one/few operations instead. 

## Package Source List in Linux
cat /etc/apt/sources.list

## Updating Available Package Lists
	updating package source list
	sudo apt-get update

## Upgrading Installed Packages
	updating software
	sudo apt-get upgrade

## Other package related tasks
	man apt-get
	<!-- remove packages no longer required -->
	sudo apt-get autoremove
	<!-- install an application called finger -->
	sudo apt-get install finger

## Discovering Packages
	https://packages.ubuntu.com/
	<!-- trusty package list -->
	https://packages.ubuntu.com/trusty/

## Using Finger
	<!-- Output all of the users currently logged into the system -->
	finger
	<!-- Additional information about a specific user -->
	finger vagrant

## etc/passwd
	cat /etc/passwd
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
	<!-- add password (I just used student as the pw for this project -->
	confirm adduser by using the finger command:
	finger student

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
	























