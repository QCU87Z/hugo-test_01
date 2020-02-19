+++
title = "This site and Docker"
description = ""
tags = [
    "hugo",
    "docker",
]
date = "2022-02-19"
categories = [
    "docker",
    "blog",
]
menu = "main"
+++

## How to run this container

```bash
docker run -d --rm --name "hugooo" -p 1313:1313 -v /srv/hugo/src:/src qcu87z/hugo
```

## How to build this container

```bash 
docker build -t qcu87z/hugo:latest .
```

Note the docker file is a fork from <https://github.com/jojomi/docker-hugo>

I have simplified the run.sh script though to suit the workflow of this project
