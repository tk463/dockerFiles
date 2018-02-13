FROM openjdk:jre-alpine

LABEL maintainer="tk463"

ARG APP_VERSION=2017.3
ARG APP_BUILD=2774

LABEL version="${APP_VERSION}.${APP_BUILD}"

ENV APP_NAME=upsource \
    APP_PORT=8080 \
    APP_UID=900 \
    APP_PREFIX=/opt \
    APP_DISTNAME="upsource-${APP_VERSION}.${APP_BUILD}"

ENV APP_USER=${APP_NAME} \
    APP_DIR=${APP_PREFIX}/${APP_NAME} \
    APP_DISTFILE="${APP_DISTNAME}.zip"

RUN mkdir ${APP_PREFIX} && \
    chmod 777 ${APP_PREFIX} && \
    addgroup -g ${APP_UID} -S ${APP_USER} && \
    adduser -S -u ${APP_UID} -h ${APP_DIR} ${APP_USER} ${APP_USER} && \
    chown -R ${APP_USER}:${APP_USER} ${APP_DIR}

WORKDIR ${APP_PREFIX}

RUN wget -q https://download.jetbrains.com/${APP_NAME}/${APP_DISTFILE} && \
    unzip -q ${APP_DISTFILE} -x */internal/java/* && \
    rm ${APP_DISTFILE} && \
    mv ${APP_DISTNAME}/* ${APP_NAME} && \
    rm -r ${APP_DISTNAME} && \
    chown -R ${APP_USER}:${APP_USER} ${APP_DIR}

USER ${APP_USER}
WORKDIR ${APP_DIR}

RUN bin/upsource.sh configure \
    --backups-dir ${APP_DIR}/backups \
    --data-dir    ${APP_DIR}/data \
    --logs-dir    ${APP_DIR}/log \
    --temp-dir    ${APP_DIR}/tmp \
    --listen-port ${APP_PORT} \
    --base-url    http://localhost/

EXPOSE ${APP_PORT}
VOLUME ${APP_DIR}/backup ${APP_DIR}/conf ${APP_DIR}/data ${APP_DIR}/log ${APP_DIR}/tmp
ENTRYPOINT [ "bin/upsource.sh" ]
CMD [ "run" ]
