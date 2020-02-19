+++
title = "This site and Docker"
description = ""
tags = [
    "hugo",
    "docker",
]
date = "2020-02-19"
categories = [
    "docker",
    "blog",
]
menu = "main"
+++

This will allow you to build and run container running Hugo.

```Dockerfile
FROM alpine:latest


# config
ENV HUGO_VERSION=0.64.1
#ENV HUGO_TYPE=
ENV HUGO_TYPE=_extended

COPY ./run.sh /run.sh
ENV HUGO_ID=hugo${HUGO_TYPE}_${HUGO_VERSION}
ADD https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/${HUGO_ID}_Linux-64bit.tar.gz /tmp
RUN tar -xf /tmp/${HUGO_ID}_Linux-64bit.tar.gz -C /tmp \
    && mkdir -p /usr/local/sbin \
    && mv /tmp/hugo /usr/local/sbin/hugo \
    && rm -rf /tmp/${HUGO_ID}_linux_amd64 \
    && rm -rf /tmp/${HUGO_ID}_Linux-64bit.tar.gz \
    && rm -rf /tmp/LICENSE.md \
    && rm -rf /tmp/README.md

RUN apk add --update git asciidoctor libc6-compat libstdc++ \
    && apk upgrade \
    && apk add --no-cache ca-certificates \
    && chmod 0777 /run.sh

VOLUME /src
VOLUME /output

WORKDIR /src
CMD ["/run.sh"]

EXPOSE 1313
```

To update the version of Hugo adjust the HUGO_VERSION to the version required and rebuild the image.

## How to build this container

```bash
docker build -t qcu87z/hugo:latest .
```

## How to run this container

```bash
docker run -d --rm --name "hugooo" -p 1313:1313 -v /srv/hugo/src:/src qcu87z/hugo
```

> Note the docker file is a fork from <https://github.com/jojomi/docker-hugo>

I have simplified the run.sh script though to suit the workflow of this project i.e. no extra features. The watch feature might need to be exposed again.

### WIP

The only bits left are to have the container destoried and recreated when a new copy of this repo is pushed to the master branch.
