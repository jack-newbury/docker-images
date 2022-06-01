FROM circleci/runner:launch-agent

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
RUN sudo curl -L https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 -o /tmp/jq-linux64 && sudo chmod a+x /tmp/jq-linux64 && sudo mv /tmp/jq-linux64 /usr/bin/jq

## Install boto3
RUN pip3 install boto3

## Intall AWS CLI
RUN sudo curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"; \
    sudo unzip awscliv2.zip; \
    sudo ./aws/install -i /usr/local/aws-cli -b /usr/local/bin

RUN sudo useradd ansible ;\
    sudo mkdir -p /home/ansible/.ssh/; \
    sudo echo "ansible ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers; \
    sudo chown -R circleci:circleci /home/ansible/; \
    sudo chown -R circleci:circleci /home/circleci