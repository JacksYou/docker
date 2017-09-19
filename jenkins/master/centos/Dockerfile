FROM centos:latest
USER root

# Use root home as working dir
RUN rm /bin/sh ; ln -s /bin/bash /bin/sh
RUN cd ~

# Import CentOS 7 keys for default repos
RUN rpm --import https://www.centos.org/keys/RPM-GPG-KEY-CentOS-7

# Update and install basic build tools
RUN yum -y update
RUN yum -y install epel-release
RUN yum -y install git wget deltarpm

# Install all Java and JVM related packages
ARG JAVA_VERSION= 8
RUN yum -y install openjdk-${JAVA_VERSION}-jdk-headless jetty8 groovy2

# All dependencies for N-BodyShop codes
ENV DEPENDENCIES make gcc gcc-c++ gcc-devel openmpi openmpi-devel openssh
RUN yum -y install ${DEPENDENCIES}

# Install supervisord
RUN yum -y install python python-setuptools
RUN easy_install supervisor ; chmod +x /usr/bin/supervisord
ADD ../supervisord.conf /etc/supervisor/

# Install charm++ according to ChaNGa build wiki
RUN git clone https://charm.cs.illinois.edu/gerrit/charm
RUN cd charm ; git checkout charm-6.7.1 ; \
    ./build charm++ netlrts-linux-x86_64 --with-production ; \
    ./build ChaNGa  netlrts-linux-x86_64 --with-production ; \
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

RUN mkdir -p /usr/share/jenkins/ref

ARG JENKINS_MIRROR=http://mirrors.jenkins.io/war-stable/latest/jenkins.war
RUN wget ${JENKINS_MIRROR} -O /usr/share/jenkins/jenkins.war

RUN groupadd -g ${gid} ${group}
RUN useradd -d "$JENKINS_HOME" -u ${uid} -g ${gid} -m -s /bin/bash ${user}
RUN chown -R jenkins "$JENKINS_HOME" /usr/share/jenkins/ref

EXPOSE ${http_port}
EXPOSE ${agent_port}

ENTRYPOINT ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
