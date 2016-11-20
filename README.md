# R-from-PHP-class
Integrating R scripts into a LAMP stack

## 1a. Starting from scratch, setting up a LAMP framework with Digital Ocean
There are numerous other tutorials available for running & installing R on a remote server. This overview is designed specifically for installing R on a Ubuntu/Linux server and integrating R scripts into an existing LAMP stack. 

This tutorial assumes that individuals have a basic understanding of PHP, Apache and MySQL. A LAMP stack is a for a web server using Linux, Apache, MySQL, and PHP. Linux is an operating system, Apache is web-server software, MySQL is a database, and PHP is a programming language. This is a common stack combination and supports many CMS applications. In this tutorial we will go over options for integrating R scripts securely and effectively into this framework. 

For individuals starting completely from scratch, I would recommend creating a droplet within Digital Ocean or some related web service. Then install Apache, PHP and MySQL and finally integrate R into the mix. The following sequence of tutorials can followed to get everything set up (*expect this to take ~ 2 - 3 hours if this is your first time ever setting up a virtual server*).

|  |              DIGITAL OCEAN TUTORIALS                                                         | 
|-------------------------|-----------------------------------------------------------------------| 
| 1                       | [How To Connect To Your Droplet with SSH](https://www.digitalocean.com/community/tutorials/how-to-connect-to-your-droplet-with-ssh)                               | 
| 2                       | [Initial Server Setup with Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04)                                | 
| 3                       | [How To Install Linux, Apache, MySQL, PHP (LAMP) stack on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04) | 
| 4                       | [A Basic MySQL Tutorial](https://www.digitalocean.com/community/tutorials/a-basic-mysql-tutorial)                                                | 
| 6                       | [How To Set Up a Host Name with DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-host-name-with-digitalocean)               |
| 7                       | [How To Secure Apache with Let's Encrypt on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-16-04)               | 
| 8                       | [How To Install and Secure phpMyAdmin on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-16-04)                  |


## 1b. Integrating R into an existing project LAMP
For individuals starting completely form






