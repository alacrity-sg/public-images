FROM debian:stable-slim as builder
WORKDIR /
ARG NODEJS_VERSION="v14.17.6"
RUN apt update && apt upgrade -y
RUN apt-get install -y wget xz-utils
RUN wget -O nodejs.tar.xz  https://nodejs.org/dist/${NODEJS_VERSION}/node-${NODEJS_VERSION}-linux-x64.tar.xz
RUN tar xf nodejs.tar.xz
RUN mv node-${NODEJS_VERSION}-linux-x64 node


FROM python:3.9-slim-buster
ENV PATH="/opt/node/bin:$PATH"
WORKDIR /
COPY --from=builder node /opt/node
RUN apt update && apt upgrade -y
RUN apt install -y git curl jq
RUN pip3 install awscli

ENV DOCKERVERSION=20.10.12
ENV SERVERLESS_VERSION=2.72.2

RUN curl -fsSLO https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKERVERSION}.tgz \
  && tar xzvf docker-${DOCKERVERSION}.tgz --strip 1 \
                 -C /usr/local/bin docker/docker \
  && rm docker-${DOCKERVERSION}.tgz
RUN npm i -g serverless@${SERVERLESS_VERSION}


CMD ["sls"]


