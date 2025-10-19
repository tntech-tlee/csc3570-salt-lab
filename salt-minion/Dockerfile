FROM ubuntu:22.04
LABEL org.opencontainers.image.authors="hjimenez"
ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=1
SHELL ["/bin/bash", "-c"]

RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections
RUN apt-get update -y && apt-get install apt-utils wget gnupg -y
RUN wget --no-check-certificate https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public && \
    apt-key add public && \
    rm public

RUN apt-get update -y && \
    apt-get install salt-minion salt-ssh -y && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN sed -i "s|#master: salt|master: salt-master|g" /etc/salt/minion
RUN cat /etc/salt/minion

ENTRYPOINT ["salt-minion", "-l", "debug"]
