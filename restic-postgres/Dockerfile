FROM debian:12-slim
LABEL maintainer="Olivier Wenger <olivier@wngr.ch>"

ARG DEBIAN_FRONTEND=noninteractive

SHELL ["/bin/bash", "-eo", "pipefail", "-c"]

# Make sure the image is up to the latest versions
# Install the system dependencies
## restic            Backup to Infomaniak's SwissBackup
## postgresql-client Handling of the DB
## rsync             Allow roadrunner to pull backups
## util-linux        ionice: nicer backup to SwissBackup
RUN apt-get update && \
  apt-get -y upgrade && \
  apt-get install -y --no-install-recommends gnupg curl ca-certificates && \
  echo "deb http://apt.postgresql.org/pub/repos/apt bullseye-pgdg main" > /etc/apt/sources.list.d/pgdg.list && \
  curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - && \
  apt-get update \
  && apt-get install -y --no-install-recommends \
  restic \
  postgresql-client-14 \
  rsync \
  file \
  util-linux \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

# Clean-up
RUN apt-get autoremove -y \
  && rm -rf /var/lib/apt /var/cache/apt

COPY root /

ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

USER daemon

ENTRYPOINT ["/usr/local/bin/backup"]


