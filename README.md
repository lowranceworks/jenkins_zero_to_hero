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

## Introduction and Installation
Create a local DNS for your Jenkins server (Windows)
- edit C:\Windows\System32\drivers\etc
- add a line containing the ip address of the server and the name you want to assign it
(e.g. 192.168.50.159 jenkins.local )

## Getting Started with Jenkins

## Jenkins and Docker

## Jenkins & AWS

## Jenkins and Ansible

## Jenkins & Security 
Install the Role-based Authorization Strategy plugin
- Enables user authorization using a Role-Based strategy. 
- Roles can be defined globally or for particular jobs or nodes selected by regular expressions.

Enable the Roles 
- Manage Jenkins -> Configure Global Security -> Authorization -> Enable Role-Based Strategy -> Apply




## Tips and Tricks
Environment variables can be used in any of your jobs \
https://www.jenkins.io/doc/pipeline/tour/environment/ \
You can create custom environment variables, these are known as custom global environment variables \
 \
Jenkins Cron \
Configure job -> Build triggers -> Build Periodically -> Schedule -> 0 1 * * * \
\
Trigger jobs from external sources \
- Create a user and configure permissions
- Dashboard -> Manage Jenkins -> Manage users -> Create user -> Manage Jenkins -> Manage and Assign Roles -> Manage Roles -> Create new rule (trigger-jobs) -> Grant rights: (Overall read) (Job Read and Build) -> Save -> Assign trigger-jobs role to user -> Save 


 
## Troubleshooting

unable to ssh into remote_host container
- delete the file containing the old remote_host id
```
rm /var/jenkins_home/.ssh/known_hosts
```
Githooks throwing 403 forbidden errors 
- There's a chance your githooks won't trigger correctly with 403 erros. This is due to a jenkins major upgrade, which modified something called CSRF in Jenkins, that protects you against DOS attacks.
- https://jenkins.io/doc/upgrade-guide/2.176/#SECURITY-626

Resolution:
* Install a plugin named Strict Crumb Issue
* Go to Manage Jenkins -> Configure Global Security -> CSRF Protection.
* Select Strict Crumb Issuer.
* Click on Advanced.
* Uncheck the Check the session ID box.
* Save it.

## Disclaimer

Private keys should NEVER be uploaded to a repository.
- In this case it doesn't matter because this is just a lab for learning and these keys are for internal containers. 
- Without the EC2 key, you cannot access the container even if you have the key to them. 

