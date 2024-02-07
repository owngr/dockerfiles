FROM debian:12-slim
LABEL authors="fitz@wngr.ch"

ARG DEBIAN_FRONTEND=noninteractive

SHELL ["/bin/bash", "-eo", "pipefail", "-c"]

# Make sure the image is up to the latest versions
# Install the system dependencies
## restic            Backup to Infomaniak's SwissBackup
## mariadb-client    Handling of the DB
## rsync             Allow roadrunner to pull backups
## util-linux        ionice: nicer backup to SwissBackup
RUN apt-get update && \
  apt-get -y upgrade && \
  apt-get install -y --no-install-recommends gnupg curl ca-certificates && \
  apt-get update \
  && apt-get install -y --no-install-recommends \
  restic \
  mariadb-client=1:10.11.4-1~deb12u1 \
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
