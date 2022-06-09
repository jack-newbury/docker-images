FROM cimg/go:1.18.2

USER root

RUN apt update -y && apt install gpg lsb-release ca-certificates wget jq qemu-user-static binfmt-support docker.io -y
RUN wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | tee /usr/share/keyrings/hashicorp-archive-keyring.gpg >/dev/null
RUN echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list
RUN apt update -y && apt install vault -y
RUN setcap cap_ipc_lock= /usr/bin/vault