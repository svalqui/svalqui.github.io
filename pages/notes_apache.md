# install

```
sudo apt install apache2
```

# remove
```
sudo apt remove apache2
```
# Initial default site
```
/var/www/html
```
if you want to store multiple sites you need to create a direcory for each
```
sudo mkdir /var/www/my-site.org
sudo vi /var/www/my-site.org/index.html
```
and each needs it own configuration
```
sudo vi /etc/apache2/sites-available/my-site.org.conf
```
## default page
```
/var/www/html/index.html
```
To set it to use other default page, edit `/etc/apache2/mods-enabled/dir.conf` 
```
DirectoryIndex my-report.txt
```
and check the file is in `/var/www/html`

# configuration 
## main
```
/etc/apache2/apache2.conf
```
## configuration per site
```
/etc/apache2/sites-available/
```
on each configuration file
```
sudo vi /etc/apache2/sites-available/my-site.org.conf
```
you need to define the server name
```
ServerName my-site.org
```
## default configuration example
```
/etc/apache2/sites-enabled/000-default.conf
```

# enabling/disabling a site
```
sudo a2ensite my-site.org.conf
sudo a2dissite my-site.org.conf
```
# enabled sites
```
/etc/apache2/sites-enabled/
```
# Logs apache
```
/var/log/apache2/error.log
/var/log/apache2/access.log
```
live access
```
tail -f /var/log/apache2/access.log
```

# managing the service
```
sudo systemctl start apache2
sudo systemctl stop apache2
sudo systemctl reload apache2
```

# ssl certificates
```
SSLCertificateFile      /etc/ssl/certs/
SSLCertificateKeyFile /etc/ssl/private/
```

