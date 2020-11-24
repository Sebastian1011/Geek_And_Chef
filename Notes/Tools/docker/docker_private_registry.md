# docker private registry

This doc assume the followings:
* your server url is : https://www.example.com 
* your dns, routing , server allow access the registry's host on port 443
* you have a certificate from an CA
* your computer can access docker hub
* docker is running on ubuntu18.04
* your domain serve multi servers


1. install docker on your computer and server
    
    [reference the docker docs](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

2. obtain a certificate from ca
    
    [Let's encrypt](https://letsencrypt.org/)

3. install nginx

    [docs](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04) 


4. deploy a registry server

    * run registry docker : `docker run -d -p 5000:5000 --restart=always --name registry registry:2`
    * obtain certificate
        ```
        $ sudo apt-get update
        $ sudo apt-get install software-properties-common
        $ sudo add-apt-repository ppa:certbot/certbot
        $ sudo apt-get update
        $ sudo apt-get install python-certbot-nginx 
        $ sudo certbot --nginx 
        ```
        this will add https configs to nginx.conf

    * edit nginx.conf
        ```
        server {
                server_name www.example.com;

                charset utf-8;

                listen 443 ssl; # managed by Certbot
                ssl_certificate /etc/letsencrypt/live/www.example.com/fullchain.pem; # managed by Certbot
                ssl_certificate_key /etc/letsencrypt/live/www.example.com/privkey.pem; # managed by Certbot
                include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
                ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
                client_max_body_size 0;
                chunked_transfer_encoding on;

                location / {
                        proxy_pass http://127.0.0.1:5000;
                }
        }
        ```
    * reload nginx.conf `sudo nginx -s reload`

5. push image to private registry

    * `docker pull ubuntu:18.04`
    * `docker tag ubuntu:18.04 www.example.com/my-image`
    * `docker push www.example.com/my-image`
    * now you can pull your image: `docker pull www.example.com/my-image`
