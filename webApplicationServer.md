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

	nano /var/www/html/index.html
	sudo nano /var/www/html/index.html

	This will open the index.html file in the embedded nano editor. Use the cursor to nvigate the file and the keyboard shortcuts (i.e., ctrl+O, ctrol+K) tht appear at the bottom of the editpr page) to modify the file.
	Then click ctrl+O to save and ctrl+X to exit the editor. 
	Refresh your localhost:8080 page in the browser to check the changes have taken place.

	sudo vim /var/www/html/index.html
	Vim Cheat Sheet
	https://vim.rtorr.com/

## Synced Folders in Vagrant Machines
	https://github.com/magicquadrant/vagrant-synced-folders
	
	make the following changes to the Vagrantfile:
	Vagrant.configure("2") do |config|  
  	config.vm.synced_folder "html/", "/var/www/html"

  	Create an html folder in the project root folder
  	Create an index.html file in your html folder. 

## Installing mod_wsgi
	Apache Configuration 
	https://httpd.apache.org/docs/2.2/configuring.html

	Ask Ubuntu Thread
	https://askubuntu.com/questions/256013/apache-error-could-not-reliably-determine-the-servers-fully-qualified-domain-n

	check mod_wsgi.png image

	sudo apt-get install libapache2-mod-wsgi

	Now we need to configure Apache to handle requests using the WSGI module. We can do this by editing the 
	/etc/apache2/sites-enabled/000-default.conf file.

	cd /etc/apache2
	ls -F
	cd sites-enabled/
	ls
	(You will see a 000-default.conf file there)
	sudo nano 000-default.conf

	The above file tells Apache how to respond to requests, 
	where to find the files for a particular site and much more. 
	We can read up on everything this file can do within the Apache documentation.

	Apache Documentation:
	https://httpd.apache.org/docs/2.2/configuring.html

	For now, add the following line at the end of the 
	<VirtualHost *:80> block, right before the closing </VirtualHost> line: 
	WSGIScriptAlias / /var/www/html/myapp.wsgi

	Finally, restart Apache with the following command:
	sudo apache2ctl restart

	H00558: apache2: Could not reliably determine the server's fully qualified domain name, using 10.0.2.15. Set the 'ServerName' directive globally to suppress this message

	Solution to suppress error message:
	https://askubuntu.com/questions/329323/problem-with-restarting-apache-2

	# create the configuration file in the "available" section
	echo "ServerName localhost" | sudo tee /etc/apache2/conf-available/servername.conf
	
	# enable it by creating a symlink to it from the "enabled" section
	sudo a2enconf servername
	
	# restart the server	
	sudo service apache2 reload
	sudo service apache2 restart
	sudo apache2ctl restart
	(No error messages occuring now)

## First WSGI Application
	Test apache configuration by writing WSGI python code.
	sudo nano /var/www/html/myapp.wsgi

			def application(environ, start_response):
		    status = '200 OK'
		    output = 'Hello Kenderson!'

		    response_headers = [('Content-type', 'text/plain'), ('Content-Length', str(len(output)))]
		    start_response(status, response_headers)

		    return [output]

    Go to localhost:8080 in browser to check if WSGI application is working

## Installing PostgreSQL

	Most web applications require persistent data storage, typically using a database server. Let's install PostgreSQL to server our data using the command: 
			sudo apt-get install postgresql

	Since I am installing my web server and database server on the same machine, I do not need to modify my firewall settings. 

	My web server will communicate with the database via an internal mechanism that does not cross the boundaries of the firewall. If I were installing my database on a separate machine, I would need to modify the firewall settings on both the web server and the database server to permit these requests.

	Exercise
	Update the /var/www/html/myapp.wsgi application so that it successfully connects to a database, queries a table for data and presents that piece of data rather than the text Hello World! 
	Create a table and populate it with data, then query it from the app.

### Research for Exercise Notes

