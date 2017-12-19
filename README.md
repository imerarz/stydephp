Aplication based on PHP 7.1 and Laravel 5.5

To enable permission on selinux is necesary run the following scripts

```
semanage fcontext -a -t httpd_sys_rw_content_t "/var/www/html/styde/storage(/.*)?"
semanage fcontext -a -t httpd_sys_rw_content_t "/var/www/html/styde/bootstrap/cache(/.*)?"
restorecon -Rv /var/www/html/
```

I enable the following property for comunication with the database
```
semanage boolean -m httpd_can_network_connect_db --on
```

https://www.linkedin.com/pulse/selinux-laravel-framework-rodrigo-alvares

I using virtual host on fedora httpd, because of this i created the following file */etc/httpd/conf.d/styde.conf*

```
<VirtualHost *:80>
    DocumentRoot /var/www/html/styde/public
    ServerName styde.local
    ServerAdmin edvramirezr@gmail.com
    <Directory  /var/www/html/styde/public>
	Options Indexes FollowSymLinks MultiViews
        AllowOverride All
        Order allow,deny
        allow from all
        Require all granted
    </Directory>
</VirtualHost>
```
The servername specified in the previous file need to be created on the file */etc/hosts*

The permission on the directory *storage* and *bootstrap/cache* need to be changed to owned by the apache user to able to work correctly

```
chown -R apache:apache /var/www/html/styde/storage
chown -R apache:apache /var/www/html/styde/bootstrap/cache
```