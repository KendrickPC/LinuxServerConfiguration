# Web Application Server

	Use Apache HTTP server to respond to HTTP requests and serve a static webpage
	http://httpd.apache.org/

	Configure Apache to hand-off specific requests to Python providing the ability to develop dynamic websites

	Setup PostgreSQL and write a simple Python application that generates a data-driven website
	https://www.postgresql.org/

## Installation Instructions:
	cd into project folder. 
	vagrant up
	vagrant ssh
	sudo apt-get install apache2

	Test if the apache2 install worked by opening a web browser and going to 
	localhost:8080

## Synced Folders in Vagrant Machines
	https://github.com/magicquadrant/vagrant-synced-folders
	
	make the following changes to the Vagrantfile:
	Vagrant.configure("2") do |config|  
  	config.vm.synced_folder "html/", "/var/www/html"

  	Create an html folder in the project root folder
  	Create an index.html file in your html folder. 


