FROM jenkins/jenkins

USER root

# Install ansible
RUN apt update -y && \
    apt install software-properties-common -y && \
    apt-add-repository --yes --update ppa:ansible/ansible -y && \
    apt install ansible -y

# Install Docker

RUN curl -fsSL https://get.docker.com | bash

# Compose

RUN curl -L "https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose

RUN usermod -aG docker jenkins

USER jenkins

