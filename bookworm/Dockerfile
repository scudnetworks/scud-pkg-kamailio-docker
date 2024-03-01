FROM debian:bookworm
LABEL org.opencontainers.image.authors Victor Seva <linuxmaniac@torreviejawireless.org>

# Important! Update this no-op ENV variable when this Dockerfile
# is updated with the current date. It will force refresh of all
# of the base images and things like 'apt-get update' won't be using
# old cached versions when the Dockerfile is built.
ENV REFRESHED_AT 2024-02-05

RUN rm -rf /var/lib/apt/lists/* && apt-get update
RUN echo MIRRORSITE=http://deb.debian.org/debian > /etc/pbuilderrc
RUN DEBIAN_FRONTEND=noninteractive apt-get install -qq --assume-yes  clang pbuilder 

RUN apt-get install -y libpcre3-dev librdkafka-dev librdkafka1

RUN mkdir -p /usr/lib64/
RUN cd /usr/lib64/; ln -s /usr/lib/x86_64-linux-gnu/libssl.a libssl.a 
RUN cd /usr/lib64/; ln -s /usr/lib/x86_64-linux-gnu/libcrypto.a libcrypto.a

VOLUME /code

RUN mkdir -p /usr/local/src/pkg
COPY debian /usr/local/src/pkg/debian

# get build dependences
RUN cd /usr/local/src/pkg/ && /usr/lib/pbuilder/pbuilder-satisfydepends-experimental

# clean
RUN apt-get clean && rm -rf /var/lib/apt/lists/*
WORKDIR /code
