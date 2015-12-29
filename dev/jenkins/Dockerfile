FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color

# Install wget && java
RUN yum install -y wget java-1.7.0-openjdk

# Add jenkins repo
RUN wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo && \
	rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key && \
	yum install -y jenkins

# Install docker client
RUN yum install -y docker

# Install Git
RUN yum install -y git

EXPOSE 8080
CMD java -jar /usr/lib/jenkins/jenkins.war
