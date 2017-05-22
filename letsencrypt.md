Let's Encrypt
==========================

## 1. Install certbot

[Digital Ocean Tutorial](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04)

## 2. Make a folder for letsencrypt to work

```
sudo mkdir /var/www/{project-name}
```

## 3. Map the project folder into the virtual host

```
	location ~ /.well-known {
        allow all;
        root /var/www/{project-name}/;
    }
```

### Virtual host Example

	server {
		listen 80;
		server_name jenkins.dev24hr.com;

		location ~ /.well-known {
    	    allow all;
        	root /var/www/jenkins/;
	    }

		location / {
    	    proxy_pass http://localhost:8080;
       	}
	}

## 4. Obtain a certificate

```
sudo certbot certonly --webroot -w /var/www/{project-name}/ -d www.project-name.com
```

## 5. Generate dhparam

```
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```

this step happens just once for the whole server

## 6. Virtual host Example
	

	server {
		listen 80;
		server_name jenkins.dev24hr.com;

		return 301 https://$host$request_uri;
	}

	# HTTPS
	# sudo certbot certonly --webroot -w /var/www/jenkins/ -d jenkins.dev24hr.com
	server {
		listen 443 ssl http2;
		server_name jenkins.dev24hr.com;

		ssl_certificate /etc/letsencrypt/live/jenkins.dev24hr.com/fullchain.pem;
		ssl_certificate_key /etc/letsencrypt/live/jenkins.dev24hr.com/privkey.pem;
		ssl_dhparam /etc/ssl/certs/dhparam.pem;

		location ~ /.well-known {
    	    allow all;
        	root /var/www/jenkins/;
	    }

		location / {
    	    proxy_pass http://localhost:8080;
       	}
	}

## Renew the certificate

Follow instructions in the DigitalOcean tutorial
