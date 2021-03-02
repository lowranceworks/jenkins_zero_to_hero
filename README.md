# Jenkins, From Zero To Hero
https://www.udemy.com/course/jenkins-from-zero-to-hero/

## Project set up for Ubuntu 

- install docker
https://docs.docker.com/engine/install/ubuntu/

- install docker-compose
https://docs.docker.com/compose/install/

enable docker to start up after reboot 
```
sudo systemctl enable docker 
```

grant the ubuntu user rights to run docker
```
sudo usermod -aG docker ${USER}
```

update docker.sock permissions, restart docker, sign out and sign back in to ensure that the changes go into effect 
```
sudo chmod 666 /var/run/docker.sock && \ 
sudo systemctl restart docker && \ 
exit 
```

clone repository 
```
git clone https://github.com/lowranceworks/jenkins_zero_to_hero.git
```

grant the ubuntu user rights to the jenkins directory
```
sudo chown 1000:1000 jenkins_zero_to_hero -R
```

## How to config local DNS name to ip address (Windows)

- edit C:\Windows\System32\drivers\etc
- add a line containing the ip address of the server and the name you want to assign it
(e.g. 192.168.50.159 jenkins.local )

## Troubleshooting

unable to ssh into remote_host container
- delete the file containing the old remote_host id
```
rm /var/jenkins_home/.ssh/known_hosts
```


## Notes
- Private keys should NEVER be uploaded to a repository. In this case it doesn't matter because this is just a lab for learning and the keys are for internal containers, not EC2 instances. 

