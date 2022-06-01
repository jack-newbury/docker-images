# pinned version of the Alpine-tagged 'go' image
FROM golang:alpine

# Install SSH GIT Curl UnZip and Gettext
RUN apk update
RUN apk add git openssh unzip curl gettext

# Install latest JQ
RUN curl -L https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 -o /tmp/jq-linux64 && chmod a+x /tmp/jq-linux64 && mv /tmp/jq-linux64 /usr/bin/jq

