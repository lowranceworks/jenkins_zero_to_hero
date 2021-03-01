# Jenkins, From Zero To Hero
https://www.udemy.com/course/jenkins-from-zero-to-hero/

## Set up for Ubuntu 

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
sudo usermod -aG docker ubuntu 
````

clone repository 
```
git clone https://github.com/lowranceworks/jenkins_zero_to_hero.git
```

grant the ubuntu user rights to the jenkins directory
```
sudo chown 1000:1000 jenkins_zero_to_hero -R
```

## Troubleshooting

unable to ssh into remote_host container
- delete the file containing the old remote_host id
```
rm /var/jenkins_home/.ssh/known_hosts
```


## Notes
- Private keys should NEVER be uploaded to a repository. In this case it doesn't matter because this is just a lab for learning and the keys are for internal containers, not EC2 instances. 

