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

Starting from an existing project we will install R following this tutorial by Dean Attali (2015): [How To Set Up R on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-r-on-ubuntu-14-04).

Be sure to match your Ubuntu version with the correct R version. In this case I am usign Ubuntu 16.04, but the tutorial uses version 'trusty' rather than 'xenial'. Verify correct version [here](https://cran.rstudio.com/bin/linux/ubuntu/). 
```{r, engine='sh', count_lines}
sudo sh -c 'echo "deb http://cran.rstudio.com/bin/linux/ubuntu xenial/" >> /etc/apt/sources.list'
```



We need to figure out some information about how host and port are setup 
```{r, engine='sh', count_lines}
mysql -u root -p
```

```sql
-- Get the port number;
 SHOW GLOBAL VARIABLES LIKE 'PORT';

-- Get the host name;
SELECT @@hostname;
show variables where Variable_name like '%host%';

```





Install R libraries RMySQL and DBI on your server. You may run into some issues installing RMySQL, if so many sure you set dependacies = TRUE. Example, install.packages("foo", dependencies=TRUE).

```{r, engine='sh', count_lines}
sudo apt-get install r-cran-rmysql
sudo su - -c "R -e \"install.packages('RMySQL', repos = 'http://cran.rstudio.com/')\""
```


### Basic (but dangerous) connection with root
Even though my real IP address is something like 176.123.123.54, when I connect directly to the dataabse through RMySQL I am using a localhost connection. You might have to play around with the settigns a little bit for the first successful dbConnect(). Depending on which version of Ubunut you're using, there might be some subtle differenes. If you're really stuck try looking in your 'my.conf' file(MySQL configuration file). ``` nano /etc/mysql/my.cnf ```.

```r
library(RMySQL)
# Initial connection to database
mydb  = dbConnect(MySQL(), dbname = "mydbname", user = "myusername", password = "mypassword", host = "127.0.0.1", port=3306)
# List tables
dbListTables(mydb)
# List fields for a sample table
dbListFields(mydb, 'potluck')
# Basic select query
rs = dbSendQuery(mydb, "select * from potluck")
rs # notice this is not data, just the SQL result we have to fetch() results to send them to a dataframe.
mydata = fetch(rs, n=-1)
# Always be sure to close connections
dbDisconnect(mydb)
all_cons = dbListConnections(MySQL()) 
# List of all connections to ensure we have closed everything

# Try saving a little sample table as a csv
getwd()
setwd("/home/matt")
dir.create(paste0("/home/matt", "/my_reports"), showWarnings = FALSE)
setwd(paste0("/home/matt", "/my_reports"))
write.csv(mydata, file="mydata.csv")
quit("no")
```
You can veiw the start of the csv file we just created on Ubunut here  ```column -s, -t < mydata.csv | less -#2 -N -S ```. Just make sure you havigate down to the right directory to where you saved it first.


#### Set up RMySQL with the MySQL configuration file. 


