FROM deathwalker66/alpine

LABEL Maintainer="CatDEV <dev.catnet@gmail.com>"

### Add NGinX Repositories for Alpine
### Add Trust this Project Public Key to Trust the Packages
ADD https://nginx.org/keys/nginx_signing.rsa.pub /etc/apk/keys/nginx_signing.rsa.pub

### COPY
COPY ./PRC/profile /etc/profile
COPY SCP /SCP

### [ RUN ]
### [!]: Replace \ + && with ;\
RUN set -eux ;\
### Install Packages
    apk --no-cache add \
### pre-requisites
    openssl ;\
### Set Repository
	printf "%s%s%s\n" \
    "http://nginx.org/packages/alpine/v" \
    `egrep -o '^[0-9]+\.[0-9]+' /etc/alpine-release` \
    "/main" \
    | tee -a /etc/apk/repositories ;\
### Install NGinX
	apk --no-cache add nginx ;\
### Create a Applications Dir
	mkdir /APP  ;\
	chmod -R 777 /APP ;\
### Create a SSL Cetificades Dir
	mkdir -p /etc/nginx/ssl ;\
	mkdir -p /run/nginx ;\
	chmod -R 777 /run/nginx ;\
### Create Certificades for HTTPS
	openssl req -x509 -nodes -days 1000 -newkey rsa:2048 -keyout /etc/nginx/ssl/nginx.key -out /etc/nginx/ssl/nginx.crt \
	-subj "/C=GT/ST=Guatemala/L=Guatemala/O=CatWare Developer Division" ;\
### Logs
	mkdir -p /var/log/nginx ;\
	chown -R nginx:nginx /var/log/nginx ;\
### Set Script Dir
    chmod +x /SCP/* ;\
### Clean [!]
    /SCP/clean.sh

### [ EXPOSE ]
EXPOSE 22 80 443

### [ ENTRYPOINT ]
ENTRYPOINT ["/usr/bin/dumb-init", "--"]

### [ CMD ]
CMD ["/SCP/entrypoint.sh"]
