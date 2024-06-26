ARG BASE_IMAGE=asterisk-unimrcp-build
FROM $BASE_IMAGE

RUN apt-get update && \
    apt-get -y install \
    automake \
    autoconf \
    sudo \
    libtool \
    git \
    curl \
    wget \
    libnewt-dev \
    libssl-dev \
    libncurses5-dev \
    subversion \
    libsqlite3-dev \
    build-essential \
    libjansson-dev \
    libxml2-dev \
    tcpdump \
    rsync \
    uuid-dev && \
    apt-get autoremove -y

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get -y install tzdata
RUN apt-get -y install locales

RUN localedef -i en_US -f UTF-8 en_US.UTF-8
ENV LANG=en_US.UTF-8

COPY start.sh /opt/start.sh

ENTRYPOINT ["/opt/start.sh"]

CMD ["asterisk", "-vvvdddf", "-T", "-p"]

LABEL modules="ubuntu:22.04-asterisk:${ASTERISK_VER}"
