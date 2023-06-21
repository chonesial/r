# SSL Certificate using Certbot .

This guide provides step-by-step instructions for installing SSL certificates using Certbot. Certbot is a popular tool that automates the process of obtaining and renewing SSL certificates from Let's Encrypt.

## Content 
### - Prerequisites
### - Installation for Self 
### - Installation for Third Party Domain


## Prerequisites

Before proceeding with the installation of SSL certificates using Certbot, make sure you have the following:

- A registered domain name pointing to your server's public IP address.
- A web server (such as Apache or Nginx) installed and configured to serve your website or application.
- Administrative access (root or sudo) to the server where the web server is installed.

## Installation

Follow these steps to install Certbot and obtain SSL certificates for your domain:

1. **Install Certbot**: Start by installing Certbot on your server. The installation process may vary depending on your operating system. Use the appropriate package manager or follow the instructions specific to your distribution. For example, on Ubuntu, you can use the following commands:

```
   sudo apt update
   sudo apt install certbot
```


2. Choose the Web Server Plugin: Certbot offers plugins for various web servers. Determine which web server you are using (Apache, Nginx, 
    etc.), and select the corresponding Certbot plugin. For example, if you are using Apache, you can use the certbot-apache plugin. If 
   you are using Nginx, you can use the certbot-nginx plugin.

3. Obtain SSL Certificates: Run the Certbot command with the chosen plugin to obtain and install SSL certificates. The command typically 
   follows this format:

```
$ sudo certbot <plugin> certonly --domain <your_domain>

```
instead of plugin, use your own plugin name 

4. The Nginx configuration files are located in the /etc/nginx directory. 
   The main configuration file is usually named nginx.conf, and the server-specific configuration files can be found in the 
   sites-available directory.

```

    server {
    listen 443 ssl;
    server_name yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

    # Other SSL settings (e.g., preferred protocols, ciphers, etc.)
    ...
      }

```
   
   Add path to your privkey.pem and fullchain.pem

5. Apply these configuration ,
   First, test the configuration.
        
```
       sudo nginx -t

 ```

Restart nginx. 

7. 
