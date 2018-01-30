FROM ubuntu:latest

MAINTAINER James You <youy2@mcmaster.ca>

ENV charm_url https://charm.cs.illinois.edu/gerrit/charm

USER root
RUN apt-get -qq update
RUN apt-get install -y git wget apt-utils ssh gcc g++ make build-essential

RUN useradd -ms /bin/bash charm
WORKDIR /home/charm
