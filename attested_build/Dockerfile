FROM oe:latest

LABEL maintainer="antdl@microsoft.com"
LABEL description="Attested build image and Debian package release for CCF"

ENV DEBIAN_FRONTEND noninteractive

RUN git clone https://github.com/ad-l/CCF
WORKDIR CCF
RUN git submodule update --recursive --init

RUN apt update \
    && apt install -y ansible software-properties-common bsdmainutils dnsutils \
    && ansible-playbook getting_started/setup_vm/oe-app-dev.yml \
    && apt remove -y ansible software-properties-common \
    && apt -y autoremove \
    && apt -y clean
