FROM alpine:edge


# LABEL

LABEL org.opencontainers.image.source="https://github.com/amirulandalib/mltb-alpine"
LABEL org.opencontainers.image.description="Docker for MLTB by Anasty based on Alpine EDGE docker image"


ARG TARGETPLATFORM BUILDPLATFORM

# Setup Working Directory

WORKDIR /usr/src/app
RUN chmod 777 /usr/src/app && chmod +x /usr/src/app

ENV MEGANZSDK_DEPS="libtool curl-dev libsodium-dev c-ares-dev sqlite-dev freeimage-dev swig boost-dev libpthread-stubs zlib-dev libpq-dev clang clang-dev ccache gettext gettext-dev gawk crypto++ crypto++-dev"
ENV NORMAL_PKGS="sudo py3-wheel musl-dev musl qbittorrent python3 python3-dev busybox qbittorrent-nox py3-pip py3-lxml aria2 p7zip xz curl pv jq ffmpeg musl-locales neofetch git make g++ gcc automake zip unzip autoconf speedtest-cli mediainfo bash alpine-sdk libffi-dev py3-virtualenv libffi dpkg cmake icu-data-full"


# Installing basic packages

RUN echo -e "\e[32m[INFO]: Installing basic packages.\e[0m" && \
    apk update && apk upgrade && \
    apk add --upgrade --no-cache \
    $NORMAL_PKGS

# Installing MegaSdkC++ Build Tools

RUN echo -e "\e[32m[INFO]: Installing MegaSdkC++ Building tools.\e[0m" && \
    echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \
    echo "http://dl-cdn.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories && \
    sudo apk add --upgrade --no-cache \
    $MEGANZSDK_DEPS


# Building and Installing MegaSdkC++

ENV PYTHONWARNINGS=ignore
RUN echo -e "\e[32m[INFO]: Building and Installing MegaSdkC++.\e[0m" && \
    git clone https://github.com/meganz/sdk.git ~/home/sdk \
    && cd ~/home/sdk && rm -rf .git \
    && autoupdate -fIv && ./autogen.sh \
    && ./configure CFLAGS='-fpermissive' CXXFLAGS='-fpermissive' CPPFLAGS='-fpermissive' CCFLAGS='-fpermissive' \
    --disable-silent-rules --enable-python --with-sodium --disable-examples --with-python3 \
    && make -j$(nproc --all) \
    && cd bindings/python/ && python3 setup.py bdist_wheel \
    && cd dist && ls && pip3 install --no-cache-dir *.whl  


# Deleting MegaSdkC++ Building tools

RUN echo -e "\e[32m[INFO]: Deleting MegaSdkC++Building tools.\e[0m" && \
    sudo apk del $MEGANZSDK_DEPS


# Running Final Apk Update

RUN echo -e "\e[32m[INFO]: Running Final Apk Update.\e[0m" && \
    sudo apk update && apk upgrade


# Setup Language Environments
ENV LANG="en_US.UTF-8" LANGUAGE="en_US:en" LC_ALL="en_US.UTF-8"

SHELL ["/bin/bash", "-c"]

COPY . .
