FROM secure-compliance-solutions/gvmk-gvmd-base:${VERSION} AS builder

ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=C.UTF-8

# Install prerequisites for optional features
RUN apt-get update && apt-get install -y \
    xsltproc \
    xmlstarlet \
    zip \
    rpm \
    fakeroot \
    dpkg \
    nsis \
    gnupg \
    wget \
    sshpass \
    openssh-client \
    rsync \
    socat \
    snmp \
    postgresql \
    postgresql-contrib \
    python3 \
    python3-lxml \
    smbclient \
    gpgsm \
    gnutls-bin \
    xml-twig-tools \
  && rm -rf /var/lib/apt/lists/*

RUN if [ "$INSTALL_PDFLATEX" = "yes" ]; then \
    apt-get update \
    apt-get install -y texlive-latex-extra --no-install-recommends; \
    apt-get install -y texlive-fonts-recommended; \
    rm -rf /var/lib/apt/lists/*; \
  fi

ARG RUN_FEED_SYNC
RUN \
  if [ "$RUN_FEED_SYNC" = "yes" ]; then \
    greenbone-feed-sync --type GVMD_DATA; \
    greenbone-feed-sync --type SCAP; \
    greenbone-feed-sync --type CERT; \
  fi


ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib

COPY docker-entrypoint-gvmd.sh docker-entrypoint.sh
ENTRYPOINT [ "/docker-entrypoint.sh" ]
CMD [ "--listen=0.0.0.0", "--port=5050" ]











