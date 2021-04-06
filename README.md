# Jenkins, From Zero To Hero
https://www.udemy.com/course/jenkins-from-zero-to-hero/

## Index

### Section 1:
- . 
### Section 2:
- .

### Section 15: CI/CD + Jenkins Pipeline + Docker + Maven
- Deploy: Execute the deploy script in the remote machine
- Deploy: Add your deploy script to Jenkinsfile
- Create a Git Repository to store your scripts and the code for the app
- Create the Jenkins Pipeline. Finally!
- Modify the path when mounting Docker volumes
- Create the Registry Passwrod in Jeknkins
- Add the private ssh key to the Jenkins container
- Add post actions to Jenkinsfile
- Execute your Pipeline manually
- Create a Git Hook to automatically trigger your Pipeline
- Start the CI/CD process by commiting the new code to Git! 

### Troubleshooting

### Disclaimers 



## Section 1: Project set up for Ubuntu 

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
We will use Jenkins & Maven together for continuous integration 

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


## Pipelines and Jenkins file
There are two different ways to define Jenkins as code: 
- Declarative pipeline (easy way, can be used with bash, Python or any programming language)
- Scripted pipeline (requires knowledge of groovy or Ruby)

**Read the documentation below** \
https://www.jenkins.io/doc/book/pipeline/

- Note that you must have the Jenkins Pipeline plugin installed (comes with suggested plugins)


## Jenkins file examples

### **simple pipeline**  
```
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'building...'
            }
        }

        stage('Test') {
            steps {
                echo 'testing...'
            }
        }

        stage('Deploy') {
            steps {
                echo 'deploying...'
            }
        }
    }
}
```

### **running multi-line shell commands in pipelines**
```
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'echo "building..."'
                sh '''
                echo "by the way, I can do more than one command at a time..."
                ls -lah 
                '''
            }
        }
    }
}
```

### **add retry and timeout instructions to your pipelines**
```
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                retry(3) {
                    sh 'echo "hello world"'
                }

                timeout(time:3, unit:'SECONDS') {
                    sh 'sleep 5'
                }
            }
        }
    }
}
```

### **add environment variables to your pipelines**
```
pipeline {
    agent any

    environment {
        FIRST_NAME = 'Josh'
        LAST_NAME = 'Lowrance'
    }

    stages {
        stage('Build') {
            steps {
                sh 'echo "my name is $FIRST_NAME $LAST_NAME"'
            }
        }
    }
}
```
How would you use existing environment variables defined on Jenkins?
- see below, there's a function for credentials so there's likely one for existing environment variables
 

### **credentials in pipelines**

import secrets defined in jenkins (manage jenkins -> manage credentials -> global credentials)

```
pipeline {
    agent any

    environment {
        SECRET = credentials('AWS_DEV_KEY')
    }

    stages {
        stage('Build') {
            steps {
                sh 'echo $SECRET'
            }
        }
    }
}
```

  
### **post actions in pipelines**
```
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'echo "FAIL!"; exit:1'
            }
        }
    }

    post {
        always {
            echo 'I will always be executed :D'
        }

        success {
            echo 'I will only run if this is successful'
        }

        failure {
            echo 'I will only run if this fails'
        }

        unstable {
            echo 'I will only run if this is unstable'
        }
    }
}
```

## Section 15: CI/CD + Jenkins Pipeline + Docker + Maven

### How to install docker inside of a docker container

Why would you want to do this? 
- to run commands like docker build and docker run - these commands are required for your CI\CD pipeline 
- this is a normal process 

Why do we map the var/run/docker.sock files?
-  this allows the container to call the same file being used on the host. Docker can now work with images and containers defined on the host.

review docker-compose.yml:
```
version: '3'
services:
  jenkins-2:
    container_name: jenkins-2
    image: jenkins/docker
    restart: always
    build:
      context: pipeline
    ports:
      - "8080:8080"
    volumes:
      - "$PWD/jenkins_home:/var/jenkins_home"
      - /var/run/docker.sock:/var/run/docker.sock 
    networks:
      - net
```
the build context points to the pipeline dir \ 
review the run instructions on the Dockerfile inside the pipeline directory that will define our new container with docker: `cat pipeline/Dockerfile` \
- root user installs everything
- docker install for Debian (because jenkins/jenkins runs on Debian)
- `usermod -aG docker jenkins` to add the jenkins user to the docker group
- switch to jenkins user 

run `docker-compose up -d` to spin up your containers \
remote into your new jenkins container with `docker exec -ti jenkins-2 bash` \
verify the os `cat etc/*release*` \
run `docker ps` inside the new jenkins container and you'll receive the following message: 
```
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/json: dial unix /var/run/docker.sock: connect: permission denied
```

this is because the jenkins user cannot access the sock file on the docker host. \
to fix this, exit the container and run `sudo chown 1000:1000 /var/run/docker.sock` on your docker host


### Define steps for your pipeline 
review the Jenkinsfile `cat pipeline/Jenkinsfile` \
this Jenkinfile defines 4 stages

### Build: Create a Jar for your Maven app using docker

A few things need to happen: 
- build\run a docker container using the maven alpine image  
- set the working directory to /app 
- map volume from (container: ./app) <-> (host: ./java-app/target/) to persist the jar file
- map volume from (container: ./root/.m2) <-> (host: $PWD/volume/root/.m2:/root/.m2) to persist dependencies required by maven
- run the command to package the application into a .jar file 
- the container should stop be removed after it's finished running 

the following command will accomplish all of these things: 
```
docker run --rm -v $PWD/java-app:/app -v $PWD/volume/root/.m2:/root/.m2 -w /app maven:3-alpine mvn -B -DskipTests clean package
```
- the '--rm' is a flag that tells docker to remove the container once it has stopped (commonly used in CI\CD pipelines) \
- the dependencies are saved on the host (volume/root/.m2) so that you can quickly rebuild the jar file anytime a commit is made 

try deleting the jar file and then running the docker command again: 

```
sudo rm $PWD//java-app/target/my-app-1.0-SNAPSHOT.jar && \
docker run --rm -v $PWD/java-app:/app -v $PWD/volume/root/.m2:/root/.m2 -w /app maven:3-alpine mvn -B -DskipTests clean package
```

### write a bash script to automate the jar file creation
view the shell script below

```
cat ./jenkins/build/maven.sh
```

"$@" represents a parameter to be used in bash 
- with this parameter, you can tell maven what to do with the following command:

```
./jenkins/build/maven.sh mvn -B -DskipTests clean package && \
ls ./java-app/target/my-app-1.0-SNAPSHOT.jar
```

### create a dockerfile and build an image with your jar 
view the dockerfile below
```
cat ./jenkins/build/Dockerfile 
```

this will copy anything ending in .jar to the container \
note that the jar file doesn't exist in our current directory, so we need to copy it to this directory (why not specify where the file is?... you'll see why later.) \
go into the build directory and copy the jar file manually:
```
cd ./jenkins/build/ && \
cp ../../java-app/target/my-app-1.0-SNAPSHOT.jar . && \
ls 
```

now that the jar file has been copied over, let's build our image: 
```
docker build -t test . 
```

you'll find this image has the jar file inside the app directory
```
docker run --rm -w /app/ test ls
```

now when you run this container, it's running your application:
```
$ docker run -d test 
fca0d8623df24021863600adb78dee4d12171aa0e674cfc2f8ea799e3657fb46
$ docker logs -f fca
Hello World!
```
congratulations, your application is now containerized! 

### create a docker compose file to automate the image build process 
view docker-compose.yml:
```
cat docker-compose.yml
```

$BUILD_TAG is a global environment variable in jenkins that can be used in the docker-compose.yml file
- this variables is what allows us to version the images that we're creating
- each image will be tagged and versioned with the jenkins job number 
- you could do this with the commit hash generataed by git    


if you try docker-compose build without setting the build tag variable, you'll receive an error. \
for now, use this command to set the variable, build the image and view it :
```
export BUILD_TAG=1 && \
docker-compose build && \
docker images | grep -w app 
```

update the BUILD_TAG variable and create a new image:
```
export BUILD_TAG=2 && \
docker-compose build && \
docker images | grep -w app 
```
jenkins will build the images and version the old ones for you  

### write a bash script to automate the docker image creation process 

remember how we had to copy and paste the jar file into the build directory \
we also had to run docker-compose build in our build directory

we will call a bash script to do these tasks for us:
```
cat ./jenkins/build/build.sh 
```

### add your scripts to the jenkinsfile 

go back to your pipeline directory:
```
cd ../../
```

remember that you can build a new jar file with the following:
```
./jenkins/build/maven.sh mvn -B -DskipTests clean package
```

remember that you can build a docker image with the newly generated jar file:
```
./jenkins/build/build.sh 
```

now how could we tell jenkins to do this for us? 
- the Build stage can run the shell scripts that we have created: 

```
cat ./Jenkinsfile
```
How does the jenkins container access these files? (they're under the pipelines directory which is not mapped to the container)
- 

### learn to test your code with maven and docker 

use 'mvn test' to test our code with maven \
remember that maven runs in docker and that we parameterized our command eariler \
provide the parameter 'mvn test' as an argument:  
```
docker run --rm -v $PWD/java-app:/app -v $PWD/volume/root/.m2:/root/.m2 -w /app maven:3-alpine mvn test
```

surefire-reports will be created after completing the test
```
ls java-app/target/surefire-reports/
```

### create a bash script to automate the test process

review the script: 
```
ls -l ./jenkins/test/maven.sh && cat ./jenkins/test/maven.sh
```
### add your test script to the jenkinsfile 

.


## 15. CI/CD + Jenkins Pipeline + Docker + Maven

### Learn how to install Docker inside of a Docker Container
. \
. 
```
.
```

### Define the steps for your Pipeline
. \
. 
```
.
```

### Build: Create a Jar for your maven App using Docker
. \
. 
```
.
```

### Build: write a bash scirpt to automate the JAr creation
. \
. 
```
.
```

### Build: Create a Docker Compose file to uatomate the image build process
. \
. 
```
.
```

### Build: Write a bash script to automate the Docker Image creation process
. \
. 
```
.
```

### Build: Add your scripts to the Jenkinsfile
. \
. 
```
.
```

### Test: Learn how to test your code using Maven and Docker 
. \
. 
```
.
```

### Test: Create a bash script to automate the test process
. \
. 
```
.
```


### Test: Add your test script to Jenkinsfile
. \
. 
```
.
```


### Create a remote machine to deploy yuour containerized app
. \
. 
```
.
```


### Push: Create a Repoistory in Docker Hub
. \
. 
```
.
```


### Push: Learn how to Push/Pull Docker images to your Repository
. \
. 
```
.
```


### Push: Write a bash script to automate the push process
. \
. 
```
.
```


### Deploy: Transfer some vairables to the remote machine
. \
. 
```
.
```


### Deploy: Deploy your appilcation on the remote machine manually 
. \
. 
```
.
```


### Deploy: Transfer the deployment script to the remote machine
. \
. 
```
.
```

### Deploy: Execute the deploy script in the remote machine
. \
. 
```
.
```

### Deploy: Add your deploy script to Jenkinsfile
. \
. 
```
.
```

### Create a Git Repository to store your scripts and the code for the app
. \
. 
```
.
```

### Create the Jenkins Pipeline. Finally!
. \
. 
```
.
```

### Modify the path when mounting Docker volumes
. \
. 
```
.
```

### Create the Registry Passwrod in Jeknkins
. \
. 
```
.
```

### Add the private ssh key to the Jenkins container
. \
. 
```
.
```

### Add post actions to Jenkinsfile
. \
. 
```
.
```

### Execute your Pipeline manually
. \
. 
```
.
```

### Create a Git Hook to automatically trigger your Pipeline
. \
. 
```
.
```

### Start the CI/CD process by commiting the new code to Git! 
. \
. 
```
.
```


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

## Disclaimers

Private keys should NEVER be uploaded to a repository.
- In this case it doesn't matter because this is just a lab for learning and these keys are for internal containers. 
- Without the EC2 key, you cannot access the container even if you have the key to them. 

