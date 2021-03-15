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

## Jenkins & Email
If you select 'install suggested plugins' while setting up jenkins, you'll find a plugin called mailer in Plugin Manager 
Can be configured Amazon SES or a gmail account \
make sure port 465 is open otherwise you will receive an error during setup \ 
\

**Features**
- email notifications when builds fail
- email notifications when builds are successful again after failure 

## Jenkins & Maven
Jenkins & Maven work well together \
We will use Jenkins & Maven together for continuous integration \

**Objectives**
 - create a CI solution for when a developers commits to a particular branch

 1. install the maven integration plugin 
 2. ensure the git client & git plugin are installed (comes with suggested plugins)
 3. create a new job that will clone this git repo using jenkins: https://github.com/jenkins-docs/simple-java-maven-app 
 - when you run the build, it will successfully clone the repository in the workspace (everything happens in the workspace) 
 - /var/jenkins_home/workspace/maven-job 
 4. build a jar using maven and the code that you've cloned
 - define a maven installation in jenkins (manage -> global tool configuration -> add maven -> save)
 - go into the maven job -> add a build step -> Invoke top-level Maven targets -> select the maven tool you added ->
 5. 

 ## Introduction Jenkins & DSL
**Create jobs with code** \

install the Job DSL plugin 

**what is a seed job?** 
- a job that creates jobs 
- the seed job can be thought of as the parent job for all of the jobs that will be created 

**how do we create a seed job?** \ 
after installing the Job DSL plugin, take the following steps: \
create a new job -> select add build step -> select Process Job DSLs 
- you will either point to a DSL script or write the DSL script inside the module 
- the script is written in groovy

**understand the DSL structure** \
we will create our first DSL job \

**DSL documentation**
https://jenkinsci.github.io/job-dsl-plugin/ 




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

