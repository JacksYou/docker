FROM ubuntu:latest

LABEL maintainer <youy2@mcmaster.ca>

ENV CHARM_REPO https://charm.cs.illinois.edu/gerrit/charm

USER root
RUN apt-get -qq update
RUN apt-get install -y apt-utils git gcc g++ gfortran make build-essential dh-autoreconf

WORKDIR /
RUN git clone ${CHARM_REPO}
