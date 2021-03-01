# Jenkins, From Zero To Hero
https://www.udemy.com/course/jenkins-from-zero-to-hero/

## Set up for Ubuntu 

- install docker
```
sudo apt-get remove docker docker-engine docker.io containerd runc ;
sudo apt-get update && \
sudo apt-get install && \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common \
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \
sudo apt-key fingerprint 0EBFCD88 && \
sudo add-apt-repository && \
sudo apt-get update && \
apt-get install docker-ce docker-ce-cli containerd.io
```

- install docker-compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
sudo chmod +x /usr/local/bin/docker-compose
```

- grant the ubuntu user rights to the jenkins directory and run docker


## Notes
- Private keys should NEVER be uploaded to a repository. In this case it doesn't matter because this is just a lab for learning and the keys are for internal containers, not EC2 instances. 

