FROM secure-compliance-solutions/gvm-base:${VERSION} AS builder
# Install Greenbone Vulnerability Manager (GVMD)
ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=C.UTF-8

ENV gvmd_version="v21.4.0"

# Install development libraries
RUN apt-get update && apt-get install -y gcc cmake gnutls-bin pkg-config xsltproc postgresql-server-dev-11 \
    libglib2.0-dev libgnutls28-dev libpq-dev libical-dev libgpgme-dev libradcli-dev \
    libgnutls28-dev libhiredis-dev libssh-gcrypt-dev

RUN mkdir /build && \
    cd /build && \
    wget --no-verbose https://github.com/greenbone/gvmd/archive/$gvmd_version.tar.gz && \
    tar -zxf $gvmd_version.tar.gz && \
    cd /build/*/ && \
    mkdir build && \
    cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release .. && \
    make && \
    make install && \
    cd / && \
    rm -rf /build

FROM ubuntu:20.10

# Install runtime libraries
RUN apt-get update && apt-get install -y \
    libc6 \
    libglib2.0 \
    libgnutls30 \
    libgnutlsxx28 \
    libgpgme11 \
    libhiredis0.14 \
    libical3 \
    libpq5 \
    libradcli4 \
    libssh-gcrypt-4 \
  && rm -rf /var/lib/apt/lists/*

COPY sync-greenbone-feed-data.sh .

COPY --from=builder /usr/local /usr/local
