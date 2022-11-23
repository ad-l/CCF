FROM antdl/oe:latest

LABEL maintainer="antdl@microsoft.com"
LABEL description="Attested build image and Debian package release for CCF"

ENV DEBIAN_FRONTEND noninteractive

RUN git clone https://github.com/ad-l/CCF
WORKDIR CCF
RUN git submodule update --recursive --init

RUN apt update \
    && apt install -y ansible software-properties-common bsdmainutils dnsutils \
    && apt satisfy -y "cmake (>= 3.16)" \
    && ([ ! -f /usr/local/bin/cmake ] || rm -f /usr/local/bin/cmake ) \
    && ansible-playbook getting_started/setup_vm/oe-app-dev.yml \
    && apt remove -y ansible software-properties-common \
    && apt -y autoremove \
    && apt -y clean

RUN mkdir build && cd build && \
    cmake -GNinja -DCOMPILE_TARGET=sgx -DCMAKE_INSTALL_PREFIX=/opt/ccf_sgx -DVERBOSE_LOGGING=OFF -DUNSAFE_VERSION=OFF  .. && \
    ninja && \
    ninja install && \
    cpack && \
    find . -name *.deb -type f -exec sha256sum {} \;

