FROM alpine:edge


# LABEL

LABEL org.opencontainers.image.source="https://github.com/amirulandalib/mltb-alpine"
LABEL org.opencontainers.image.description="Docker for MLTB by Anasty based on Alpine EDGE docker image"


ARG TARGETPLATFORM BUILDPLATFORM

# Setup Working Directory

WORKDIR /usr/src/app
RUN chmod 777 /usr/src/app && chmod +x /usr/src/app


# Installing basic packages

RUN uname -m

RUN echo -e "\e[32m[INFO]: Installing basic packages.\e[0m" && \
    apk update && apk upgrade && \
    apk add --upgrade --no-cache \
    sudo py3-wheel musl-dev musl python3 python3-dev busybox musl-locales \
    qbittorrent-nox py3-pip py3-lxml aria2 p7zip xz curl pv jq ffmpeg musl-locales parallel \
    neofetch git make g++ gcc automake zip unzip autoconf speedtest-cli mediainfo bash \
    alpine-sdk libffi-dev py3-virtualenv libffi dpkg cmake icu-data-full nodejs coreutils npm \
    && npm install -g localtunnel kill-port


# Installing Build Tools

RUN echo -e "\e[32m[INFO]: Installing Building tools.\e[0m" && \
    echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \
    echo "http://dl-cdn.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories && \
    sudo apk add --upgrade --no-cache \
    libtool curl-dev libsodium-dev c-ares-dev sqlite-dev freeimage-dev swig boost-dev \
    libpthread-stubs zlib-dev libpq-dev clang clang-dev ccache gettext gettext-dev \
    gawk crypto++ crypto++-dev


# Install Cloudflared Tunnel

RUN echo -e "\e[32m[INFO]: Installing Cloudflareed Tunnel.\e[0m" && \
    curl -L https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-$(cpu=$(uname -m).deb;\
    if [[ "$cpu" == "x86_64" ]]; then echo "amd64";\
    elif [[ "$cpu" == "armv7l" ]]; then echo "arm";\
    elif [[ "$cpu" == "aarch64" ]]; then echo "aarch64";\
    else echo $cpu; fi) && \
    sudo dpkg -i --force-architecture cloudflared-linux-$(cpu=$(uname -m).deb\
    if [[ "$cpu" == "x86_64" ]]; then echo "amd64";\
    elif [[ "$cpu" == "armv7l" ]]; then echo "arm";\
    elif [[ "$cpu" == "aarch64" ]]; then echo "aarch64";\
    else echo $cpu; fi)
    


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


RUN echo -e "\e[32m[INFO]: Installing GLIBC.\e[0m" && \
    wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub && \
    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.35-r0/glibc-bin-2.35-r0.apk && \
    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.35-r0/glibc-i18n-2.35-r0.apk && \
    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.35-r0/glibc-dev-2.35-r0.apk && \
    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.35-r0/glibc-2.35-r0.apk && \
    apk add glibc-bin-2.35-r0.apk glibc-i18n-2.35-r0.apk glibc-dev-2.35-r0.apk glibc-2.35-r0.apk && \
    rm -rf *.apk *.deb


# Running Final Apk Update

RUN echo -e "\e[32m[INFO]: Running Final Apk Update.\e[0m" && \
    sudo apk update && apk upgrade


# Setup Language Environments
ENV LANG="en_US.UTF-8" LANGUAGE="en_US:en" LC_ALL="en_US.UTF-8"

RUN echo -e "\e[32m[INFO]: Generating GLIBC EN Locales.\e[0m" && \
    /usr/glibc-compat/bin/localedef -i en_US -f UTF-8 en_US.UTF-8

SHELL ["/bin/bash", "-c"]
