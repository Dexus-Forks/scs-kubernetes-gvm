FROM ubuntu:20.10
# FROM secure-compliance-solutions/gvmk-lib:${VERSION} AS builder

# Install Greenbone Security Assistant (GSA)
ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=C.UTF-8

ENV gsa="v21.4.0"

RUN apt-get update && apt-get install -y cmake gcc gettext git gnutls-bin libmicrohttpd-dev \
    nodejs npm python-polib python3 libglib2.0-dev libgpgme-dev libhiredis-dev libmicrohttpd-dev \
    libmicrohttpd-dev libmicrohttpd12 libradcli-dev libssh-gcrypt-dev libxml2-dev


RUN mkdir /build && \
    cd /build && \
    wget --no-verbose https://github.com/greenbone/gsa/archive/$gsa.tar.gz && \
    tar -zxf $gsa.tar.gz && \
    sed -i 's/300000/90000000/g' /build/*/gsa/src/gmp/gmpsettings.js && \
    find /build/ -type f -exec sed -i 's/timeout: 30000/timeout: 9000000/g' {} \; && \
    find /build/ -type f -exec sed -i 's/expect(settings.timeout).toEqual(30000)/expect(settings.timeout).toEqual(9000000)/g' {} \; && \
    cd /build/*/ && \
    mkdir build && \
    cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release .. && \
    make && \
    make install && \
    cd / && \
    rm -rf /build

