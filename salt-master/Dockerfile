FROM ubuntu:22.04
LABEL org.opencontainers.image.authors="hjimenez"
ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=1
SHELL ["/bin/bash", "-c"]

RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections
RUN apt-get update -y && apt-get install apt-utils wget gnupg -y
RUN wget --no-check-certificate https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public && \
    apt-key add public && \
    rm public

RUN echo "deb https://packages.broadcom.com/artifactory/saltproject-deb/ stable main" > /etc/apt/sources.list.d/saltstack.list
RUN apt-get update -y 

RUN apt-get install salt-master salt-minion salt-ssh salt-syndic salt-cloud salt-api -y && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN sed -i "s|#auto_accept: False|auto_accept: True|g" /etc/salt/master

ENTRYPOINT ["salt-master", "-l", "debug"]
