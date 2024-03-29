FROM secure-compliance-solutions/gvm-base:${VERSION} AS builder

# Install Greenbone Security Assistant (GSA)
ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=C.UTF-8

ENV gsa_version="v21.4.0"

RUN apt-get update && apt-get install -y cmake gcc gettext git gnutls-bin libmicrohttpd-dev \
    nodejs npm python-polib python3 libglib2.0-dev libgpgme-dev libhiredis-dev libmicrohttpd-dev \
    libmicrohttpd-dev libmicrohttpd12 libradcli-dev libssh-gcrypt-dev libxml2-dev


RUN mkdir /build && \
    cd /build && \
    wget --no-verbose https://github.com/greenbone/gsa/archive/$gsa_version.tar.gz && \
    tar -zxf $gsa_version.tar.gz && \
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


# Release image
FROM ubuntu:20.10

# Install runtime libraries
RUN apt-get update && apt-get install -y \
    libglib2.0 \
    libgnutls30 \
    libgnutlsxx28 \
    libgpgme11 \
    libhiredis0.14 \
    libmicrohttpd12 \
    libradcli4 \
    libssh-gcrypt-4 \
    libxml2 \
    python-polib \
    gettext \
  && rm -rf /var/lib/apt/lists/*


COPY --from=builder /usr/local /usr/local

ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib:/usr/lib

ADD docker-entrypoint-gsad.sh docker-entrypoint.sh
ENTRYPOINT [ "/docker-entrypoint.sh" ]
CMD [ "--http-only", "--listen=0.0.0.0", "--port=8080" ]

