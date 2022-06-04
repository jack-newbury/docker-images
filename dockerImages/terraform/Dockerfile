FROM python:3.9-alpine

ARG TERRAFORM_VERSION=1.2.2
ARG JQ_VERSION=1.6
ARG TERRAGRUNT=v0.37.1
ENV VERSION 1.10.3

ADD https://releases.hashicorp.com/vault/${VERSION}/vault_${VERSION}_linux_amd64.zip /tmp/
ADD https://releases.hashicorp.com/vault/${VERSION}/vault_${VERSION}_SHA256SUMS      /tmp/
ADD https://releases.hashicorp.com/vault/${VERSION}/vault_${VERSION}_SHA256SUMS.sig  /tmp/

WORKDIR /tmp/

RUN apk --update add --virtual verify gpgme \
    && gpg --keyserver keyserver.ubuntu.com --recv-key 0x72D7468F \
    && gpg --verify /tmp/vault_${VERSION}_SHA256SUMS.sig \
    && apk del verify \
    && cat vault_${VERSION}_SHA256SUMS | grep linux_amd64 | sha256sum -c \
    && unzip vault_${VERSION}_linux_amd64.zip \
    && mv vault /usr/local/bin/ \
    && rm -rf /tmp/* \
    && rm -rf /var/cache/apk/*

WORKDIR /

## Install tflint
RUN apk add --no-cache ca-certificates openssh git bash unzip openssl curl wget unzip

RUN curl -L https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 -o /tmp/jq-linux64 && chmod a+x /tmp/jq-linux64 && mv /tmp/jq-linux64 /usr/bin/jq

RUN apk add --no-cache --virtual .build_deps build-base libffi-dev \
    && pip install --no-cache-dir -U checkov awscli \
    && apk del .build_deps

## Install Terraform
RUN cd tmp && \
    wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip && unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip -d /usr/bin

## Install Terragrunt
RUN wget -O /usr/local/bin/terragrunt https://github.com/gruntwork-io/terragrunt/releases/download/${TERRAGRUNT}/terragrunt_linux_amd64 && chmod +x /usr/local/bin/terragrunt
