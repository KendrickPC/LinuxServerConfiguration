<!-- Finished Setting Up AWS Ubuntu Server Instance 06/15/2018 -->
<!-- Check xip.io -->

# Amazon Lightsail

## Get started on Lightsail
We're recommending Amazon Lightsail for this project. If you prefer, you
can use any other service that gives you a publicly accessible Ubuntu Linux
server. But Lightsail works pretty well and it's what we've tested.

## There are a few things you need to do when you create your server instance.

[x] 1. Log in!
First, log in to Lightsail. If you don't already have an Amazon Web Services 
account, you'll be prompted to create one.

    Amazon Web Services login page.

[x] 2. Create an instance.
Once you're logged in, Lightsail will give you a friendly message with a robot
on it, prompting you to create an instance. A Lightsail instance is a Linux
server running on a virtual machine inside an Amazon datacenter.

Lightsail screenshot: No instances
When you have no instances, Lightsail gives you a picture of an orange robot
and suggests that you create an instance.

[x] 3. Choose an instance image: Ubuntu
Lightsail supports a lot of different instance types. An instance image is a
particular software setup, including an operating system and optionally
built-in applications.

For this project, you'll want a plain Ubuntu Linux image. There are two
settings to make here. First, choose "OS Only" (rather than "Apps + OS").
Second, choose Ubuntu as the operating system.

Lightsail setup: Pick your instance image (Ubuntu)
When you create an instance, Lightsail asks what kind you want.
For this project, choose an "OS Only" instance with Ubuntu.

[x] 4. Choose your instance plan.
The instance plan controls how powerful of a server you get. It also
controls how much money they want to charge you. For this project, the
lowest tier of instance is just fine. And as long as you complete the
project within a month and shut your instance down, the price will be zero.

Lightsail setup: Pricing options. (Choose $5/month with first month free.)
Lightsail's options for instance pricing.
For this project, pick the lowest one to get free-tier access.

Be aware: If you enable additional features in Lightsail, you may be charged
extra for them.

[x] 5. Give your instance a hostname.
Every instance needs a unique hostname. You can use any name you like, as
long as it doesn't have spaces or unusual characters in it. Your instance's
name will be visible to you and to the project reviewer.

Lightsail setup: Setting a name for your instance.
I've named my instance silly-name-here.

[x] 6. Wait for it to start up.
It may take a few minutes for your instance to start up.

Lightsail: Instance starting.
While your instance is starting up, Lightsail shows you a grayed-out display.

Lightsail: Instance running.
Once your instance is running, the display gets brighter.

[x] 7. It's running; let's use it!
Once your instance has started up, you can log into it with SSH from your
browser.

The public IP address of the instance is displayed along with its name.
In my lightsail instructions file, it's (example public) 18.182.27.32.

Note: When you set up OAuth for your application, you will need a DNS name
that refers to your instance's IP address. You can use the xip.io service
to get one; this is a public service offered for free by Basecamp. For
instance, the DNS name (example public) 18.182.27.32.xip.io refers to the
server above.

Lightsail: Main page for an instance.
The main page for my silly-name-here instance.
The big orange "Connect using SSH" button is the next step.

Explore the other tabs of this user interface to find the Lightsail firewall
and other settings. You'll need to configure the Lightsail firewall as one
step of the project.

When you SSH in, you'll be logged as the ubuntu user. When you want to execute
commands as root, you'll need to use the sudo command to do it. Review sudo
here if you need a refresher!

Lightsail: SSH window.
An SSH window logged into the server instance.
From here, it's just like any other Linux server.

8. Project time.
Now that you have a working instance, you can get right into the project!
