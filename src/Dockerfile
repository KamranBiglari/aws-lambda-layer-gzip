FROM public.ecr.aws/lambda/provided:al2
#
# versions
#
USER root
RUN mkdir -p /opt
WORKDIR /opt
#
# tools
#

RUN yum update -y \
    && yum install -y zip unzip wget tar gzip which pkgconfig \
    && yum group install -y "Development Tools"

#
# redis cli
#

RUN curl https://ftp.gnu.org/gnu/gzip/gzip-1.13.zip -o gzip-1.13.zip
RUN unzip gzip-1.13.zip
RUN cd gzip-1.13/ && ./configure && make  \
    && make install

# organize for self-contained usage

RUN mv $(eval which gzip) /opt/gzip

# cleanup

#
# create the bundle
#

RUN cd /opt \
    &&  zip -9 -j ../layer.zip /opt/gzip \
    && echo "/layer.zip is ready" \
    && ls -alh /layer.zip;

RUN /opt/gzip --version
WORKDIR /
ENTRYPOINT ["/bin/bash"]
