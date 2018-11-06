# Codering - Prototyping a PHP-runtime base image for running actions

# Note: this could easily be generated on-the-fly out of a template, to allow
# for the client to pick a specific PHP version, install deps, etc.

# Note: consider using openwhisk/action-php-v7.2 as skeleton, it has error
# handling and (I guess) composer support already baked-in.

FROM openwhisk/dockerskeleton:latest as skeleton
FROM php:7.2-cli-alpine3.8

# @see https://github.com/apache/incubator-openwhisk-runtime-docker/blob/master/core/actionProxy/Dockerfile
RUN apk upgrade --update \
  && apk add --no-cache bash perl jq zip git curl wget openssl ca-certificates sed openssh-client \
  && apk add --no-cache python2-dev py2-pip \
  && update-ca-certificates \
  && apk add --no-cache --virtual .build-deps bzip2-dev gcc libc-dev \
  && pip install --upgrade pip setuptools six \
  && pip install --no-cache-dir gevent==1.3.6 flask==1.0.2 \
  && apk del .build-deps
# TODO: if need be, install PHP runtime's dependencies (PHPUnit, etc.)

ENV FLASK_PROXY_PORT 8080
ENV PROXY_ALLOW_REINIT 1

RUN mkdir -p /action
#COPY --from=skeleton /actionProxy /actionProxy
RUN mkdir -p /actionProxy
COPY actionproxy.py /actionProxy/

CMD ["/bin/bash", "-c", "cd actionProxy && python -u actionproxy.py"]

