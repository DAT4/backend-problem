# backend-problem

I have a problem when setting up the SSL. It says that probably there is a problem with the firewall

```bash
➜  ~mongodb sudo certbot --nginx -d backend.mama.sh -d www.backend.mama.sh
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator nginx, Installer nginx
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for backend.mama.sh
http-01 challenge for www.backend.mama.sh
Waiting for verification...
Cleaning up challenges
Failed authorization procedure. backend.mama.sh (http-01): urn:ietf:params:acme:error:connection :: The server could not connect to the client to verify the domain :: Fetching http://backend.mama.sh/.well-known/acme-challenge/eJhL6PoP2TDxdw0C4sjC5javBeCvWn08ugB9PRr7YJ0: Timeout during connect (likely firewall problem), www.backend.mama.sh (http-01): urn:ietf:params:acme:error:connection :: The server could not connect to the client to verify the domain :: Fetching http://www.backend.mama.sh/.well-known/acme-challenge/WODpVIq9fr275lYISyaoCV-OPLYzRExZPr68DvOZJMs: Timeout during connect (likely firewall problem)

IMPORTANT NOTES:
 - The following errors were reported by the server:

   Domain: backend.mama.sh
   Type:   connection
   Detail: Fetching
   http://backend.mama.sh/.well-known/acme-challenge/eJhL6PoP2TDxdw0C4sjC5javBeCvWn08ugB9PRr7YJ0:
   Timeout during connect (likely firewall problem)

   Domain: www.backend.mama.sh
   Type:   connection
   Detail: Fetching
   http://www.backend.mama.sh/.well-known/acme-challenge/WODpVIq9fr275lYISyaoCV-OPLYzRExZPr68DvOZJMs:
   Timeout during connect (likely firewall problem)

   To fix these errors, please make sure that your domain name was
   entered correctly and the DNS A/AAAA record(s) for that domain
   contain(s) the right IP address. Additionally, please check that
   your computer has a publicly routable IP address and that no
   firewalls are preventing the server from communicating with the
   client. If you're using the webroot plugin, you should also verify
   that you are serving files from the webroot path you provided.
➜  ~mongodb 
```

I tried dissabeling the UFW and I made these settings when it is enables. Id did not change the output

```
➜  ~mongodb sudo ufw status
Status: active

Til                        Handling    Fra
---                        --------    ---
OpenSSH                    ALLOW       Anywhere                  
80/tcp                     ALLOW       Anywhere                  
443/tcp                    ALLOW       Anywhere                  
27017                      ALLOW       Anywhere                  
80/tcp (v6)                ALLOW       Anywhere (v6)             
443/tcp (v6)               ALLOW       Anywhere (v6)             
27017 (v6)                 ALLOW       Anywhere (v6)             
```

The configuration file in NGINX looks like this

```
➜  ~mongodb cd /etc/nginx/sites-available 
```
```
➜  sites-available cat backend.mama.sh 
server {
        listen 80;
        listen [::]:80;
        listen 443;
        listen [::]:443;

	root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        server_name backend.mama.sh www.backend.mama.sh;

	location / {
		try_files $uri $uri/ =404;
	}

}
```

If I run nmap on the domain I get this response
```
➜  sites-available nmap backend.mama.sh
```
```
Starting Nmap 7.60 ( https://nmap.org ) at 2021-02-13 12:17 CET
Nmap scan report for backend.mama.sh (130.225.170.70)
Host is up (0.00015s latency).
Not shown: 998 closed ports
PORT    STATE SERVICE
80/tcp  open  http
443/tcp open  https

Nmap done: 1 IP address (1 host up) scanned in 0.77 seconds
```
