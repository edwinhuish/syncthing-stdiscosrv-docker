FROM alpine:3.6

LABEL description="Docker container for running your own Syncthing Discovery Server."

ARG VERSION=v1.8.0

ENV BUILD_HOST=https://github.com/syncthing/discosrv/releases/download/${version}
ENV BUILD_FILE_NAME=stdiscosrv-linux-amd64-${version}
ENV STDISCOSRV_ROOT=/workspace

VOLUME $STDISCOSRV_ROOT

RUN apk add --update libc6-compat libstdc++ \
  && apk upgrade \
  && apk add --no-cache --virtual build ca-certificates wget \
  && update-ca-certificates \
  && wget -P /tmp ${BUILD_HOST}/${BUILD_FILE_NAME}.tar.gz \
  && tar xvf /tmp/${BUILD_FILE_NAME}.tar.gz -C /tmp \
  && mv /tmp/${BUILD_FILE_NAME}/stdiscosrv /usr/local/bin/stdiscosrv \
  && addgroup -g 1000 stdiscosrv \
  && adduser -D -G stdiscosrv -u 1000 stdiscosrv \
  && mkdir -p ${STDISCOSRV_ROOT} \
  && chown stdiscosrv:stdiscosrv ${STDISCOSRV_ROOT} \
  && apk del build \
  && rm "/tmp/*" -rf


WORKDIR $STDISCOSRV_ROOT

USER stdiscosrv

CMD ["sh", "-c", \
      "stdiscosrv", \
       "-db-dir=${STDISCOSRV_ROOT}/database", \
       "-cert=${STDISCOSRV_ROOT}/cert.pem", \
       "-key=${STDISCOSRV_ROOT}/key.pem"]

EXPOSE 8443
