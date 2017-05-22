NGiNX
==========================

## Install on Ubuntu

[Digital Ocean Tutorial](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04)

```
sudo service start nginx
sudo service stop nginx
sudo service restart nginx

// only for changes in config, if there are new files need to restart
sudo service reload nginx

// test new configuration
sudo nginx -t
```

Generic configuration is in `/etc/nginx`.

