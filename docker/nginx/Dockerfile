FROM nginx:alpine

RUN apk update && apk add bash

ADD ./nginx.conf /etc/nginx/nginx.conf
COPY mime.types /etc/nginx/mime.types
