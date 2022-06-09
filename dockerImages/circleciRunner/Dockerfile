FROM circleci/runner:launch-agent

ARG TARGETPLATFORM
ARG TERRAFORM_VERSION=1.2.2
ARG JQ_VERSION=1.6
ARG TERRAGRUNT=v0.37.2
ARG DOCTL_VERSION=1.76.2

SHELL ["/bin/bash", "-c"]
USER root

## Upgrade ubuntu
RUN sudo apt-get update; \
    sudo apt-get upgrade -y

## Install packages needed.
RUN sudo apt-get install gnupg curl git gettext-base rsync build-essential zlib1g-dev bzip2 libreadline-dev libssl-dev rustc docker.io software-properties-common -y

ENV PYENV_ROOT /root/.pyenv
ENV PATH $PYENV_ROOT/shims:$PYENV_ROOT/bin:$PATH
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

## Install Python.
RUN set -ex \
    && curl https://pyenv.run | bash \
    && pyenv update \
    && pyenv install 3.6.8 \
    && pyenv global 3.6.8

## Install Ansible
RUN pip3 install --upgrade pip && pip3 install ansible==2.9.9

## Install boto3
RUN pip3 install boto3 awscli

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
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/${ARCHITECTURE}/kubectl" && install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

## Install Vault
RUN curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
RUN apt-add-repository "deb [arch=${ARCHITECTURE}] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
RUN sudo apt update -y && sudo apt install vault -y