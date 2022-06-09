FROM circleci/runner:launch-agent

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
RUN sudo apt-get install gnupg curl git gettext-base rsync build-essential zlib1g-dev bzip2 libreadline-dev libssl-dev rustc docker.io -y

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

# Install latest JQ
RUN curl -L https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 -o /tmp/jq-linux64 && chmod a+x /tmp/jq-linux64 && mv /tmp/jq-linux64 /usr/bin/jq

## Install boto3
RUN pip3 install boto3

## Intall AWS CLI
RUN sudo curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"; \
    sudo unzip awscliv2.zip; \
    sudo ./aws/install -i /usr/local/aws-cli -b /usr/local/bin

## Install Terraform
RUN cd tmp && \
    wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip && unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip -d /usr/bin

## Install Terragrunt
RUN wget -O /usr/local/bin/terragrunt https://github.com/gruntwork-io/terragrunt/releases/download/${TERRAGRUNT}/terragrunt_linux_amd64 && chmod +x /usr/local/bin/terragrunt

## Intsall DOCTL
RUN wget https://github.com/digitalocean/doctl/releases/download/v${DOCTL_VERSION}/doctl-${DOCTL_VERSION}-linux-amd64.tar.gz && tar xf doctl-${DOCTL_VERSION}-linux-amd64.tar.gz && mv doctl /usr/local/bin && chmod +x /usr/local/bin/doctl

## Install Kubectl
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

## Install Vault
RUN wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg >/dev/null && echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list && sudo apt update -y && sudo apt install vault -y