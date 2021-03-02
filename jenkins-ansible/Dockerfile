FROM jenkins/jenkins

COPY remote-key var/jenkins_home/ansible/remote-key

USER root

RUN chmod 600 /var/jenkins_home/ansible/remote-key
RUN apt update
RUN apt-get install -y python3
RUN apt-get install -y ansible 

USER jenkins

