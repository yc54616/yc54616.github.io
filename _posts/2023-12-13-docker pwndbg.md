---
layout: post
title: docker pwndbg, pwntools install
date: 2023-12-10 20:00:03 +0900
categories: PWN
---

Dockerfile에 설치부분 가운데에 아래 부분을 넣으면 pwntools와 pwndbg가 오류없이 설치된다.

### docker pwndbg, pwntools install ubuntu 22.04
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

### docker pwndbg, pwntools install ubuntu 18.04
```
ENV PATH="${PATH}:/usr/local/lib/python3.6/dist-packages/bin"
ENV LC_CTYPE=C.UTF-8

RUN apt update
RUN apt install -y \
    gcc \
    git \
    python3 \
    python3-pip \
    ruby \
    sudo \
    tmux \
    vim \
    wget

# install pwndbg
WORKDIR /root
RUN git clone https://github.com/pwndbg/pwndbg
WORKDIR /root/pwndbg
RUN git checkout 2023.03.19
RUN ./setup.sh

# install pwntools
RUN pip3 install --upgrade pip
RUN pip3 install pwntools

# install one_gadget command
RUN gem install one_gadget
```