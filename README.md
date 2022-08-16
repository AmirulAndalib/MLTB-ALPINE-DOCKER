# Mirror Leech Telegram Bot [ MLTB ] Docker

# Stable Docker Image 

## [![ghcr.io](https://github.com/amirulandalib/mltb-alpine-docker/actions/workflows/github-container-deploy.yml/badge.svg)](https://github.com/amirulandalib/mltb-alpine-docker/actions/workflows/github-container-deploy.yml)
## [![hub.docker.com](https://github.com/AmirulAndalib/MLTB-ALPINE-DOCKER/actions/workflows/dockerhub-push.yml/badge.svg)](https://github.com/AmirulAndalib/MLTB-ALPINE-DOCKER/actions/workflows/dockerhub-push.yml)

---
### Dockerfile Operating System 💽 >> Alpine Linux Edge 

### Supported Architectures ✅  >> Amd64 | ArmV8 | ArmV7

### MegaSdk Repository which was used in this Dockerfile : [MegaSdkC++](https://github.com/meganz/sdk)
---

## Instructions for inserting the image On anasty17/mltb 🧰

## Warning ⚠️ >>> this dockerfile has additional components which is based on a [fixed repo : Anasty17/mltb](https://github.com/anasty17/mirror-leech-telegram-bot) so if your repo is modified and has additional modified components eg; MegaSdkRest instead of MegaSdkC++ it will not work and bot will crash . . .

## [Base dockerfile for Anasty17/MLTB](https://github.com/anasty17/mirror-leech-telegram-bot/blob/master/Dockerfile)

## Editing Instructions :

### Replace the contents Dockerfile of that like this given below 👇

---
```dockerfile
FROM ghcr.io/amirulandalib/mltb-alpine-docker:latest

# if you want to load image from dockerhub then replace the above one with this one 👇
# FROM amirulandalib/mltb-alpine-docker:latest


COPY . .

WORKDIR /usr/src/app

RUN chmod 777 /usr/src/app

RUN pip3 install --no-cache-dir -r requirements.txt

CMD ["bash", "start.sh"]
```
---



## why using Alpine As docker image when there is Ubuntu? 🤔
 
---

 ✓ Docker image is light and could be started from scratch.

 ✓ Less ram usage ... Ideal for 512-2gb ram devices.

 ✓ All pkg repos are also available in variety like Ubuntu so no shortage of dependencies here..

---


## For support contact on issues or on discussions or [here](https://t.me/kangershub)



