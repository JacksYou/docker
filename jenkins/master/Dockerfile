FROM centos:7
USER root

# Use root home as working dir
RUN cd ~

# Import CentOS 7 keys for default repos
RUN rpm --import https://www.centos.org/keys/RPM-GPG-KEY-CentOS-7

# Java version for Jenkins
ENV JAVA_VERSION java-1.8.0

# All dependencies for N-BodyShop codes
ENV DEPENDENCIES make gcc gcc-c++ gcc-devel glibc glibc-devel openmpi openmpi-devel openssh

ARG JENKINS_URL=https://updates.jenkins-ci.org/latest/jenkins.war

# Install tools + all dependencies
RUN yum -y update
RUN yum -y install git curl python3
RUN yum -y install ${JAVA_VERSION}-openjdk-headless ${JAVA_VERSION}-openjdk-devel groovy
RUN yum -y install ${DEPENDENCIES}

RUN easy_install supervisor

# Install charm++ according to ChaNGa build wiki
RUN git clone https://charm.cs.illinois.edu/gerrit/charm
RUN cd charm ; git checkout charm-6.7.1 ;
    ./build charm++ netlrts-linux-x86_64 --with-production ;
    ./build ChaNGa  netlrts-linux-x86_64 --with-production ;
    cd ..

ARG user=jenkins
ARG group=jenkins
ARG uid=1000
ARG gid=1000
ARG http_port=8080
ARG agent_port=50000

ENV JENKINS_HOME /var/jenkins_home
ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
ENV JENKINS_UC https://updates.jenkins.io

RUN curl ${JENKINS_URL} -o /usr/share/jenkins/jenkins.war

RUN chown -R ${user} "$JENKINS_HOME" /usr/share/jenkins/ref

EXPOSE ${http_port}
EXPOSE ${agent_port}

USER jenkins

ENTRYPOINT ["echo 'complete'"]
