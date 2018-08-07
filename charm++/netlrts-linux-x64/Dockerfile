FROM jacksyou/charm:base

LABEL maintainer James You <youy2@mcmaster.ca>

USER root
RUN apt-get -qq update
RUN apt-get install -y ssh openssh-server

RUN ln -s /charm/bin/charmc /bin/charmc ; \ 
    ln -s /charm/bin/charmxi /bin/charmxi ; \ 
    ln -s /charm/bin/charmrun /bin/charmrun
    
EXPOSE 22
