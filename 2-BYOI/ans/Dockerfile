FROM ubuntu:16.04

RUN apt update && apt install -y nginx

COPY index.html /var/www/tutorial/

COPY demo.conf /etc/nginx/sites-enabled/demo

ENTRYPOINT ["nginx", "-g", "daemon off;"]