set up for jenkins course:
```
sudo useradd jenkins sudo mkdir /home/jenkins/.ssh/
sudo cat /.ssh/authorized_keys > /home/jenkins/.ssh/
passwd jenkins
usermod -aG sudo jenkins ; usermod -aG root jenkins 
sudo vim /etc/sudoers 
    jenkins (ALL) (ALL)
```


when docker fails, run the following command: 
```
yum-config-manager --save --setopt=docker-ce-nightly.skip_if_unavailable=true
yum-config-manager --save --setopt=docker-ce-stable.skip_if_unavailable=true

```

set up docker on AMZN-Linux2:
```
sudo amazon-linux-extras install docker
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -a -G docker ec2-user
logout
```

connect to ec2 via ssh:
```

```