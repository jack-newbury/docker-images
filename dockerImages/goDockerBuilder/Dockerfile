FROM cimg/go:1.18.2

USER root

RUN apt update -y && apt install gpg lsb-release ca-certificates wget jq qemu-user-static binfmt-support docker.io containerd -y
RUN wget https://github.com/docker/buildx/releases/download/v0.8.2/buildx-v0.8.2.linux-amd64 -O /usr/local/lib/docker/cli-plugins/docker-buildx &&  chmod +x /usr/local/lib/docker/cli-plugins/docker-buildx
RUN wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | tee /usr/share/keyrings/hashicorp-archive-keyring.gpg >/dev/null
RUN echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list
RUN apt update -y && apt install vault -y
RUN setcap cap_ipc_lock= /usr/bin/vault
RUN sudo usermod -aG docker $(whoami)