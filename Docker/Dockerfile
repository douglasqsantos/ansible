# docker run --rm -it alpine:3.12 sh
FROM alpine:3.12

## Setting the root Directory
WORKDIR /ansible

# ENV BUILD_PACKAGES \
RUN apk update && apk upgrade && \
        apk add --no-cache bash \
        fish \
        curl \
        tar \
        vim \
        # nano \
        openssh-client \
        sshpass \
        git \
        python3 \
        # py3-boto \
        # py3-dateutil \
        # py3-httplib2 \
        # py3-jinja2 \
        # py3-paramiko \
        py3-pip \
        # py3-setuptools \
        # py3-yaml \
        py3-virtualenv \
        # py3-wheel \
        ca-certificates && \
        apk --update add --virtual build-dependencies \
        python3-dev \
        gcc \
        musl-dev \
        libffi-dev \
        openssl-dev

## Preparing the Ansible
RUN pip install --upgrade pip && \
    virtualenv venv && \
    source venv/bin/activate && \
    pip install python-keyczar docker-py boto3 botocore && \
    pip install --upgrade pip && \
    pip install ansible && \
    apk del build-dependencies && \
    rm -rf /var/cache/apk/*

CMD ["/usr/bin/fish"]

# docker run --rm -it -v "${PWD}:/ansible/PrbAnsible" -w /ansible douglasqsantos/ansible fish
# source venv/bin/activate.fish