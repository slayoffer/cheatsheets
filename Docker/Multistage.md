### Multistage example

```bash
# stage 1
FROM ubuntu:latest AS BUILD_IMAGE
RUN apt update && apt install wget unzip -y
RUN wget https://www.tooplate.com/zip-templates/2128_tween_agency.zip
RUN unzip 2128_tween_agency.zip \ # multi line
	cd 2128_tween_agency \
    tar -czf tween.tgz * \
    mv tween.tgz /root/tween.tgz

# stage 2
FROM ubuntu:latest
LABEL "project"="Marketing"
ENV DEBIAN_FRONTEND=noninteractive

RUN apt update \
	apt install apache2 git wget -y
WORKDIR /var/www/html/
COPY --from=BUILD_IMAGE /root/tween.tgz /var/www/html/
RUN cd /var/www/html/ \ 
	tar xzf tween.tgz
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"] # USE ONLY ONE PROCESS PER CONTAINER !
VOLUME /var/log/apache2
EXPOSE 80
```

### Multistage example 2

```bash
# stage 1
FROM node:14.15.Ø-alpine3.12 AS builder
ARG REACT_APP_STAGE=localhost
ARG PUBLIC_URL
RUN apk update && apk upgrade && apk add bash make git python3 curl
# set working directory
WORKDIR /app
# add /app/node_modutes/ .bin to $PATH
ENV PATH /app/node_modutes/.bin:$PATH
# install dependencies
COPY ./ctient .
RUN npm install
# build the app bundle
RUN PUBLIC_URL=$PUBLIC_URL npm run build

# stage 2
FROM bitnami/nginx:latest
WORKDIR /app
COPY ./deploy/ui.nginx.conf /opt/bitnami/nginx/conf/server_blocks/kamino.conf
COPY --from=builder /app/build .
```