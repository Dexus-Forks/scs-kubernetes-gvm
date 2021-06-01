FROM secure-compliance-solutions/gvm-base:${VERSION} AS builder
# Install Greenbone Vulnerability Manager (GVMD)

ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=C.UTF-8

ENV openvas_scanner_version="v21.4.0" \
    open_scanner_protocol_daemon="v21.4.0" \
    ospd_openvas="v21.4.0" 

RUN apt-get update && apt-get install -y \
  bison \
  cmake \
  flex \
  gcc \
  git \
  pkg-config \
  rsync \
  python3 \
  python3-defusedxml \
  python3-dev \
  python3-lxml \
  python3-paramiko \
  python3-setuptools \
  libgcrypt20-dev \
  libglib2.0-dev \
  libgnutls28-dev \
  libgnutls28-dev \
  libgpgme-dev \
  libhiredis-dev \
  libksba-dev \
  libpcap-dev \
  libradcli-dev \
  libsnmp-dev \
  libssh-gcrypt-dev


# Install Open Vulnerability Assessment System (OpenVAS) Scanner

RUN mkdir /build && \
    cd /build && \
    wget --no-verbose https://github.com/greenbone/openvas-scanner/archive/$openvas_scanner_version.tar.gz && \
    tar -zxf $openvas_scanner_version.tar.gz && \
    cd /build/*/ && \
    mkdir build && \
    cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release .. && \
    make && \
    make install && \
    cd / && \
    rm -rf /build


    # Install Open Scanner Protocol daemon (OSPd)   
RUN mkdir /build && \
    cd /build && \
    wget --no-verbose https://github.com/greenbone/ospd/archive/$open_scanner_protocol_daemon.tar.gz && \
    tar -zxf $open_scanner_protocol_daemon.tar.gz && \
    cd /build/*/ && \
    python3 setup.py install && \
    cd / && \
    rm -rf /build
    

    # Install Open Scanner Protocol for OpenVAS   
RUN mkdir /build && \
    cd /build && \
    wget --no-verbose https://github.com/greenbone/ospd-openvas/archive/$ospd_openvas.tar.gz && \
    tar -zxf $ospd_openvas.tar.gz && \
    cd /build/*/ && \
    python3 setup.py install && \
    cd / && \
    rm -rf /build


FROM ubuntu:20.10

# Install runtime libraries
RUN apt-get update && apt-get install -y \
    libc6 \
    libgcrypt20 \
    libglib2.0 \
    libgnutls30 \
    libgnutlsxx28 \
    libgpgme11 \
    libhiredis0.14 \
    libksba8 \
    libpcap0.8 \
    libpq5 \
    libradcli4 \
    libradcli4 \
    libsnmp30 \
    libssh-gcrypt-4 \
  # Install binaries used by OpenVAS
    rsync \
    snmp \
  # Install Python dependencies
    python3 \
    python3-setuptools \
    python3-paramiko \
    python3-lxml \
    python3-defusedxml \
  && rm -rf /var/lib/apt/lists/*


COPY --from=builder /usr/local /usr/local

ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib:/usr/lib
 
# Create user for running greenbone-nvt-sync
RUN useradd gvm && \
    chown -R gvm:gvm /usr/local/var/run && \
    chown -R gvm:gvm /usr/local/var/lib/openvas/plugins

COPY sync-greenbone-nvt-data.sh .

ARG RUN_NVT_SYNC
USER gvm
RUN \
  if [ "$RUN_NVT_SYNC" = "yes" ]; then \
    source /sync-greenbone-nvt-data; \
  fi

COPY docker-entrypoint-ospd-openvas.sh docker-entrypoint.sh
ENTRYPOINT [ "/docker-entrypoint.sh" ]
CMD [ "--listen=0.0.0.0", "--port=5050" ]










