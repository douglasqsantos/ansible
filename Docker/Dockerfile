# docker run --rm -it alpine:3.12 sh
FROM alpine:3.12

## Setting the root Directory
WORKDIR /ansible

# Installing the packages
RUN apk update && apk upgrade && \
        apk add --no-cache bash \
        fish \
        curl \
        tar \
        vim \
        openssh-client \
        sshpass \
        git \
        python3 \
        py3-pip \
        ca-certificates && \
        apk --update add --virtual build-dependencies \
        python3-dev \
        gcc \
        musl-dev \
        libffi-dev \
        openssl-dev

## Preparing the Ansible
RUN python3 -m venv venv_ansible && \
    source venv_ansible/bin/activate && \
    pip install --upgrade pip setuptools && \
    pip install --upgrade python-keyczar docker-py boto3 botocore && \
    pip install --upgrade ansible && \
    apk del build-dependencies && \
    rm -rf /var/cache/apk/*

## Default Command
CMD ["/usr/bin/fish"]

# docker run --rm -it -v "${PWD}:/ansible/workspace" -w /ansible ansible fish
# source venv_ansible/bin/activate.fish