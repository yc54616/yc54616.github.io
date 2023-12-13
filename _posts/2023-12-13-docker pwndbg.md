---
layout: post
title: docker pwndbg, pwntools install
date: 2023-12-10 20:00:03 +0900
categories: PWN
---

Dockerfile에 설치부분 가운데에 아래 부분을 넣으면 pwntools와 pwndbg가 오류없이 설치된다.

### docker pwndbg, pwntools install
```
RUN apt-get update -y
RUN apt-get install --reinstall -y locales
# uncomment chosen locale to enable it's generation
RUN sed -i 's/# pl_PL.UTF-8 UTF-8/pl_PL.UTF-8 UTF-8/' /etc/locale.gen
# generate chosen locale
RUN locale-gen pl_PL.UTF-8
# set system-wide locale settings 
ENV LANG pl_PL.UTF-8
ENV LANGUAGE pl_PL
ENV LC_ALL pl_PL.UTF-8

RUN apt-get install -y git 
RUN apt-get install -y vim

WORKDIR /root
RUN git clone https://github.com/pwndbg/pwndbg.git
WORKDIR /root/pwndbg
RUN ./setup.sh
RUN pip install pwntools
```