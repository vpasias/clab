FROM ubuntu:18.04

WORKDIR /unf

SHELL ["/bin/bash", "-c"]

ARG DEBIAN_FRONTEND=noninteractive

ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=DontWarn

# Use bash shell

RUN apt update -y && apt-get install -y git vim net-tools wget curl bash-completion apt-utils base-files ifupdown rsyslog tcpdump gnupg2 iproute2 socat telnet libc-ares2 libjson-c3 python-ipaddr python3-jinja2 python3-cffi python3-pycparser libbpfcc-dev lsb-release

RUN apt-get install python3-cffi python3-pycparser -y

RUN curl -sL https://deb.flexiwan.com/setup | bash -
RUN apt update -y && apt-get install -y flexiwan-router

RUN apt-get update -y

RUN apt-get clean -y

RUN apt-get update && \
    apt-get install -y libpcre3-dev apt-transport-https ca-certificates curl wget logrotate \
    libc-ares2 libjson-c3 vim procps libreadline7 gnupg2 lsb-release apt-utils && \
    rm -rf /var/lib/apt/lists/*

RUN curl -s https://deb.frrouting.org/frr/keys.asc | apt-key add -
RUN echo deb https://deb.frrouting.org/frr $(lsb_release -s -c) frr-stable | tee -a /etc/apt/sources.list.d/frr.list

RUN apt-get update && \
    apt-get install -y frr frr-pythontools && \
    rm -rf /var/lib/apt/lists/*


