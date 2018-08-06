FROM jacksyou/charm:base

LABEL maintainer James You <youy2@mcmaster.ca>

WORKDIR /charm
RUN apt-get update -y 
RUN apt-get install -y --no-install-recommends openmpi-common openmpi-bin libopenmpi-dev  

RUN ./build charm++ mpi-linux-x86_64 --with-production -j8

RUN ln -s /charm/bin/charmc /bin/charmc
RUN ln -s /charm/bin/charmxi /bin/charmxi
RUN ln -s /charm/bin/charmrun /bin/charmrun