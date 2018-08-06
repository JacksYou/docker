FROM jacksyou/charm:base

LABEL maintainer James You <youy2@mcmaster.ca>

USER root
RUN apt-get -qq update
RUN apt-get install -y ssh openssh-server

EXPOSE 22
