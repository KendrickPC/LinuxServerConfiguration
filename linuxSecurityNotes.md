# Developer Notes

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























	
