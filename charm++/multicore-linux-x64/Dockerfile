FROM jacksyou/charm:base

LABEL maintainer youy2@mcmaster.ca

WORKDIR /charm
RUN ./build charm++ multicore-linux-x86_64 --with-production -j8

RUN ln -s /charm/bin/charmc /bin/charmc
RUN ln -s /charm/bin/charmxi /bin/charmxi
RUN ln -s /charm/bin/charmrun /bin/charmrun