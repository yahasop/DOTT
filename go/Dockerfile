FROM alpine:latest

RUN apk update
RUN apk upgrade
RUN mkdir /home/app/

ADD . /home/app/
COPY . /home/app/

WORKDIR /home/app
