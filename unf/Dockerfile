FROM ubuntu:18.04

WORKDIR /unf

SHELL ["/bin/bash", "-c"]

ARG DEBIAN_FRONTEND=noninteractive

ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=DontWarn

# Use bash shell

RUN apt update -y && apt-get install -y git vim net-tools wget curl bash-completion apt-utils base-files ifupdown rsyslog tcpdump gnupg2 iproute2 socat telnet libc-ares2 libjson-c3 python-ipaddr python3-jinja2 python3-cffi python3-pycparser libbpfcc-dev lsb-release

RUN apt-get install python3-cffi python3-pycparser -y

RUN git clone https://gitlab.com/flexiwangroup/flexirouter.git && cd /unf/flexirouter && git checkout ad7e8c26a1d7d08e1d3eda40784d1c7d418565e6 && \
    git clone https://gitlab.com/flexiwangroup/flexiroutervpp.git vpp && cd vpp && git checkout 711f6d63f073f2285e4bae4cab09feaca23438c2 && \
    cd /unf/flexirouter && git clone https://gitlab.com/flexiwangroup/flexiroutersb.git vppsb && cd vppsb && \
    git checkout cf96cfe4d5e335600aedc19ce16b099f86fc7e0a && cd /unf/flexirouter && \
    cp /unf/flexirouter/scripts/prepare.sh /unf/flexirouter && cp /unf/flexirouter/scripts/vpp_build.sh /unf/flexirouter && \
    cp /unf/flexirouter/scripts/vpp_install.sh /unf/flexirouter && \
    cp /unf/flexirouter/scripts/vpp_build_and_install.sh /unf/flexirouter && \
    sed -i 's/install-ext-deps/UNATTENDED=yes install-ext-deps/' prepare.sh && sed -i 's/install-dep/UNATTENDED=yes install-dep/' prepare.sh && \
    ./prepare.sh && ./vpp_build_and_install.sh && cd /unf/flexirouter/vpp && make pkg-deb && dpkg -i /unf/flexirouter/vpp/build-root/*.deb

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


