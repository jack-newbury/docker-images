FROM python:3.10-alpine3.16

ARG TARGETPLATFORM
ARG TERRAFORM_VERSION=1.2.4
ARG JQ_VERSION=1.6
ARG TERRAGRUNT=v0.38.4
ARG DOCTL_VERSION=1.78.0

## Install tflint
RUN apk add --no-cache ca-certificates openssh git bash unzip openssl curl wget unzip vault libcap gcc build-base jq
RUN setcap cap_ipc_lock= /usr/sbin/vault

RUN pip install --no-cache-dir -U awscli

## Install Terraform
RUN cd tmp
RUN if [ "$TARGETPLATFORM" = "linux/amd64" ]; then ARCHITECTURE=amd64; elif [ "$TARGETPLATFORM" = "linux/arm64" ]; then ARCHITECTURE=arm64; else ARCHITECTURE=amd64; fi \
    && wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_${ARCHITECTURE}.zip \
    && unzip terraform_${TERRAFORM_VERSION}_linux_${ARCHITECTURE}.zip -d /usr/bin

## Install Terragrunt
RUN if [ "$TARGETPLATFORM" = "linux/amd64" ]; then ARCHITECTURE=amd64; elif [ "$TARGETPLATFORM" = "linux/arm64" ]; then ARCHITECTURE=arm64; else ARCHITECTURE=amd64; fi \
    && wget -O /usr/local/bin/terragrunt https://github.com/gruntwork-io/terragrunt/releases/download/${TERRAGRUNT}/terragrunt_linux_${ARCHITECTURE} \
    && chmod +x /usr/local/bin/terragrunt

## Intsall DOCTL
RUN if [ "$TARGETPLATFORM" = "linux/amd64" ]; then ARCHITECTURE=amd64; elif [ "$TARGETPLATFORM" = "linux/arm64" ]; then ARCHITECTURE=arm64; else ARCHITECTURE=amd64; fi \
    && wget https://github.com/digitalocean/doctl/releases/download/v${DOCTL_VERSION}/doctl-${DOCTL_VERSION}-linux-${ARCHITECTURE}.tar.gz && tar xf doctl-${DOCTL_VERSION}-linux-${ARCHITECTURE}.tar.gz && mv doctl /usr/local/bin && chmod +x /usr/local/bin/doctl

## Install Kubectl
RUN if [ "$TARGETPLATFORM" = "linux/amd64" ]; then ARCHITECTURE=amd64; elif [ "$TARGETPLATFORM" = "linux/arm64" ]; then ARCHITECTURE=arm64; else ARCHITECTURE=amd64; fi \
    && curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/${ARCHITECTURE}/kubectl" && install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

## Install Helm
RUN if [ "$TARGETPLATFORM" = "linux/amd64" ]; then ARCHITECTURE=amd64; elif [ "$TARGETPLATFORM" = "linux/arm64" ]; then ARCHITECTURE=arm64; else ARCHITECTURE=amd64; fi \
    && wget "https://get.helm.sh/helm-v3.9.2-linux-${ARCHITECTURE}.tar.gz" && tar -zxf helm-v3.9.2-linux-${ARCHITECTURE}.tar.gz && mv linux-${ARCHITECTURE}/helm /usr/local/bin && chmod +x helm