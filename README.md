# Server-Setup-Walkthrough
Customer Setup Walkthrough for quick reference


# Install LAMP Stack 
- A apache  <b> We will be using Apache2 Server in this case. </b>
- E nginx 

***

### Apache2
```
sudo apt install apache2 -y
```

### MySQL 
```
sudo apt install mysql-server -y

CREATE USER 'username'@'localhost' identified with sha256_password by 'user-password';

create database wordpress1 charset utf8mb4;
GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost' WITH GRANT OPTION;
flush privilges;
```

### PHP Installation 
```
sudo apt install php libapache2-mod-php php-mysql -y
 
```
***

# Install WordPress 
```
cd /tmp
wget https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz  
sudo cp -r wordpress /data 
sudo mv wordpress project1
sudo chown -R www-data:www-data project1
cd project1
sudo cp wp-config-sample.php wp-config.php
sudo vim wp-config.php 
    - You have to change the respective credentials

sudo mkdir /var/log/apache2/project1  //log path
```

### Virtual Hosts Setup 
```
cd /etc/apache2/sites-available 
sudo mv 000-default.conf project1.conf 
- Modify the contain of the project1.conf for eg Setup ServerName, DocumentRoot path, Custom log location. 
- sudo a2ensite project1.conf   // this is enable the project1.conf and create a symlink
- sudo a2enmod rewrite 
- sudo systemctl restart apache2 
```
Virtual Host Configuration file contain example:
``` 
    <VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com
        ServerName project1.htb
        ServerAdmin webmaster@localhost
        DocumentRoot /data/project1

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/project1/error.log
        CustomLog ${APACHE_LOG_DIR}/project1/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
        <Directory "/data/project1">
                Require all granted
        </Directory>
    </VirtualHost>
```

Tips
- To check server or os releases  
```
	cat /etc/os-release
```
- copy previous code (use !!) eg
```
	mysql -u root -p
	sudo !! 
```
- To check whether apache configuration you have changed has any syntactical issues 
```
	sudo apachectl configtest
```
tmux manual 
