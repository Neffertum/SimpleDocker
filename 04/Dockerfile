FROM ubuntu:20.04

USER root

RUN apt-get update && \
    apt-get install -y gcc && \
    apt-get install -y nginx && \
    apt-get install -y libfcgi-dev && \
    apt-get install -y spawn-fcgi

COPY ./my_server.c /home/my_server.c
COPY ./nginx/nginx.conf /etc/nginx/nginx.conf

WORKDIR /home/

CMD gcc my_server.c -lfcgi -o my_server && \
    spawn-fcgi -p 8080 ./my_server && \
    nginx -g "daemon off;" && \
    nginx -s reload