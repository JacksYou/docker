FROM ubuntu:latest
USER root

# Use root home as working dir
RUN rm /bin/sh ; ln -s /bin/bash /bin/sh
RUN cd ~

# Update and install basic build tools
RUN apt-get -y update
RUN apt-get -y install git wget apt-utils

# Install all Java and JVM related packages
ARG JAVA_VERSION=openjdk-8
RUN apt-get -y install ${JAVA_VERSION}-jdk-headless jetty8 groovy

# All apt dependencies for N-BodyShop codes
ENV DEPENDENCIES make build-essential g++ mpich libmpich-dev
RUN apt-get -y install ${DEPENDENCIES}

# Install Python and upgrade pip
RUN apt-get -y install python python-pip python-setuptools
RUN pip install --upgrade pip

# Install supervisord
RUN pip install supervisor
ADD supervisord.conf /etc/supervisor/

# Install charm++ according to ChaNGa build wiki
RUN git clone https://charm.cs.illinois.edu/gerrit/charm
RUN cd charm ; git checkout charm-6.7.1 ; \
    ./build charm++ mpi-linux-x86_64 --with-production ; \
    ./build ChaNGa  mpi-linux-x86_64 --with-production ; \
    cd /

# Install tipsy
RUN apt-get -y install libxt-dev libxaw7-dev libncurses5-dev libx11-dev
RUN git clone https://github.com/N-BodyShop/tipsy.git
RUN cd tipsy/code ; ./configure ; make ; cp tipsy /bin ; cd /

# Install SKID
RUN git clone https://github.com/N-BodyShop/skid.git
RUN cd skid ; make ; cd /

# Install SO
RUN git clone https://github.com/N-BodyShop/so.git
RUN cd so ; make ; cd /


ARG user=jenkins
ARG group=jenkins
ARG uid=1000
ARG gid=1000
ARG http_port=8080
ARG agent_port=50000

ENV JENKINS_HOME /var/jenkins_home
ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
ENV JENKINS_UC https://updates.jenkins.io

RUN mkdir -p /usr/share/jenkins/ref

ARG JENKINS_MIRROR=http://mirrors.jenkins.io/war-stable/latest/jenkins.war
RUN wget ${JENKINS_MIRROR} -O /usr/share/jenkins/jenkins.war

RUN groupadd -g ${gid} ${group}
RUN useradd -d "$JENKINS_HOME" -u ${uid} -g ${gid} -m -s /bin/bash ${user}
RUN chown -R jenkins "$JENKINS_HOME" /usr/share/jenkins/ref

EXPOSE ${http_port}
EXPOSE ${agent_port}

ENTRYPOINT ["supervisord", "-c", "/etc/supervisor/supervisord.conf"]
