<!-- @@@title:Using HTTPS@@@ -->


# Enabling https on your LogZilla server
This guide is for Ubuntu 14 and 16.  For other OS's, your file locations may vary.

### Create your ssl keys
In this example, we will store the keys in `/etc/apache2/ssl`.  You'll be prompted for a passphrase, it will only be used to create the keys, and we'll remove it a few steps down. You'll also be asked questions about the server name, location, and contact information. Fill in whatever you'd like, or just put a `.` to leave the answers blank.

	openssl genrsa -des3 -out server.key 2048
	openssl req -new -key server.key -out server.csr

Remove the passphrase from the key. 
> NOTE: If you do not remove the passphrase, Apache will not start automatically when your server is rebooted.  It will have to be started manually by entering the passphrase.

	cp server.key server.key.org   
	openssl rsa -in server.key.org -out server.key

Next, we'll need to generate a self-signed certificate

	openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt

Copy the certificate and key to the apache ssl directory:

	mkdir -p /etc/apache2/ssl && cp server.crt server.key /etc/apache2/ssl/

### Enable apache mods

	a2enmod ssl headers
	a2ensite default-ssl
### Modify the config
Edit the `/etc/apache2/sites-available/default-ssl.conf` file and add the following under the line `<IfModule mod_ssl.c>`

	RequestHeader set X-Forwarded-Proto 'https' env = HTTPS
Next, modify the DocumentRoot as follows

	DocumentRoot /home/logzilla/ui/current  
Below that, add the following entries.

	<Directory /home/logzilla/ui/current>
		Options FollowSymlinks  
		AllowOverride none  
		Require all granted  
	</Directory> 
	<Directory "/home/logzilla/src/static">  
		Options FollowSymLinks  
	    AllowOverride None  
	    Require all granted  
	</Directory>
	  ## Logging  
	  ErrorLog "/var/log/apache2/logzilla_error.log"  
	  ServerSignature Off  
	  CustomLog "/var/log/apache2/logzilla_access.log" combined  

	  ## Proxy rules  
	  ProxyRequests Off  
	  ProxyPass /ws ws://127.0.0.1:8001  

	  ProxyPassReverse ws://127.0.0.1:8001  

	  ## Rewrite rules  
	  RewriteEngine On  
	  RewriteRule ^/api/(static|sd)/?$ /api/static/docs/ [R,L]  
	  RewriteRule ^/api/static(.*) /home/logzilla/src/static$1 [L]  
	  RewriteRule ^/api(|/|/doc|/docs)$ /api/docs/ [R,L]  
	  RewriteRule ^/api(/(.*))? http://localhost:8000/api$1 [P,L]  
	  RewriteCond %{REQUEST_URI} !^/ws/  
	  RewriteCond %{LA-U:REQUEST_FILENAME} !-f  
	  RewriteCond %{LA-U:REQUEST_FILENAME} !-d  
	  RewriteRule ^ /index.html [L,NS]

Also in the `default-ssl`, you will need to modify the certificate locations under the `SSLEngine on` switch.

	SSLCertificateFile /etc/apache2/ssl/server.crt  
	SSLCertificateKeyFile /etc/apache2/ssl/server.key

>Optional: If you would like you server to redirect all requests to https, add the following line to the file `/etc/apache2/sites-available/000-default.conf`

	Redirect / https://your.server.address 

Restart Apache and test.

	service apache2 reload  



### Troubleshooting tips

If your redirect fails to work properly, make sure that there are no conflicting configuration files located in `/etc/apache2/sites-available`.
