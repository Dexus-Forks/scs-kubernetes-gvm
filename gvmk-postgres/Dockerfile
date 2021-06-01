FROM secure-compliance-solutions/gvmk-gvmd-base:${VERSION} AS builder

ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=C.UTF-8

# Install postgres
RUN apt-get update && apt-get install -y postgresql-11 \
  && rm -rf /var/lib/apt/lists/*

# Add postgres user to ssl-cert group
RUN gpasswd -a postgres ssl-cert && \
    chown -R postgres:ssl-cert /etc/ssl/private && \
    chmod 0600 /etc/ssl/private/ssl-cert-snakeoil.key

COPY docker-entrypoint-gvmd-postgres.sh docker-entrypoint.sh

USER postgres
ENTRYPOINT [ "/docker-entrypoint.sh" ]
CMD [ "-c", "config_file=/etc/postgresql/11/main/postgresql.conf" ]


