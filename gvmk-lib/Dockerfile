FROM ubuntu:20.10
# Install Libraries Module 
# For the Greenbone Vulnerability Management

ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=C.UTF-8

ENV gvm_libs="v21.4.0"

RUN apt-get update && apt-get install -y cmake gcc git pkg-config uuid-dev \
    libglib2.0-dev libgnutls28-dev libgpgme-dev libhiredis-dev libldap2-dev \
    libpcap-dev libradcli-dev libssh-gcrypt-dev libxml2-dev

    
RUN mkdir /build && \
    cd /build && \
    wget --no-verbose https://github.com/greenbone/gvm-libs/archive/$gvm_libs.tar.gz && \
    tar -zxf $gvm_libs.tar.gz && \
    cd /build/*/ && \
    mkdir build && \
    cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release .. && \
    make && \
    make install && \
    cd / && \
    rm -rf /build 