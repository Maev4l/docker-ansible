FROM python:3.7-alpine3.10

ENV ANSIBLE_VERSION 2.8.3
ENV DOCKER_CLI_VERSION 18.06.3

ENV BUILD_PACKAGES bash \
  curl \
  tar \
  openssh-client \
  sshpass \
  py3-dateutil \
  py3-httplib2 \
  py3-jinja2 \
  py3-yaml \
  ca-certificates

RUN set -x && \
    echo "==> Adding build-dependencies..."  && \
    apk --update add --virtual build-dependencies \
      gcc \
      musl-dev \
      libffi-dev \
      openssl-dev \
      python3-dev
    
RUN set -x && \
    echo "==> Upgrading apk and system..."  && \
    apk update && apk upgrade && \
    apk add --no-cache ${BUILD_PACKAGES}

RUN set -x && \
    echo "==> Adding Docker CLI..."  && \
    curl --request GET --location https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKER_CLI_VERSION}-ce.tgz --output docker.tgz && \
    tar xzvf docker.tgz --strip 1 -C /usr/local/bin docker/docker && \
    rm docker.tgz
    
RUN set -x && \
    echo "==> Adding Python runtime..."  && \
    pip install --upgrade pip && \
    pip install python-keyczar docker

RUN set -x && \
    echo "==> Installing Ansible..."  && \
    pip install ansible==${ANSIBLE_VERSION}
    
RUN set -x && \
    echo "==> Cleaning up..."  && \
    apk del build-dependencies && \
    rm -rf /var/cache/apk/*
    
RUN set -x && \
    echo "==> Adding hosts for convenience..."  && \
    mkdir -p /etc/ansible /ansible && \
    echo "[local]" >> /etc/ansible/hosts && \
    echo "localhost" >> /etc/ansible/hosts

ENV ANSIBLE_GATHERING smart
ENV ANSIBLE_HOST_KEY_CHECKING false
ENV ANSIBLE_RETRY_FILES_ENABLED false
ENV ANSIBLE_ROLES_PATH /ansible/playbooks/roles
ENV ANSIBLE_SSH_PIPELINING True
ENV PYTHONPATH /ansible/lib
ENV PATH /ansible/bin:$PATH
ENV ANSIBLE_LIBRARY /ansible/library
 
WORKDIR /ansible/playbooks