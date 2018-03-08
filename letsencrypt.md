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

## After Reinstall Ubintu

```
The default certbot cron hooks have been disabled!                                                                                                               │
                    │                                                                                                                                                                  │
                    │ The global cron hooks as provided by Ubuntu PPA packages would disrupt any custom setup for renewals possibly causing the renewals of the certificates to fail.  │
                    │                                                                                                                                                                  │
                    │ As of 0.12.0 version of the packages, the default cron hooks have been removed.  You have two options if you want to keep the existing functionality:            │
                    │                                                                                                                                                                  │
                    │   * Change the default cron job or systemd timers, and add:                                                                                                      │
                    │       --pre-hook '/bin/run-parts /etc/letsencrypt/pre-hook.d/' \                                                                                                 │
                    │       --post-hook '/bin/run-parts /etc/letsencrypt/post-hook.d/' \                                                                                               │
                    │       --renew-hook '/bin/run-parts /etc/letsencrypt/renew-hook.d/'                                                                                               │
                    │     at the end of the `certbot -q renew' command.                                                                                                                │
                    │                                                                                                                                                                  │
                    │                                                                                                                                                                  │
                    │   * Add following lines to every /etc/letsencrypt/renewal/<domain>.conf                                                                                          │
                    │     in the [renewalparams] section:                                                                                                                              │
                    │       post_hook = /bin/run-parts /etc/letsencrypt/post-hook.d/                                                                                                   │
                    │       renew_hook = /bin/run-parts /etc/letsencrypt/renew-hook.d/                                                                                                 │
                    │       pre_hook = /bin/run-parts /etc/letsencrypt/pre-hook.d/                                                                                                     │
                    │     and use the same command line options when issuing a new certificate.
                    
```
