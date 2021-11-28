## Copyright (c) 2021, Oracle and/or its affiliates.
FROM          oraclelinux:8
LABEL         maintainer="Stephen Balousek<stephen.balousek@oracle.com>"

ARG           INSTALLER
RUN           : ${INSTALLER:?}

ENV           OGG_HOME               "/u01/ogg"
ENV           OGG_DEPLOYMENT_HOME    "/u02"
ENV           OGG_TEMPORARY_FILES    "/u03"
ENV           JAVA_HOME              "/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-2.el8_5.x86_64"
ENV           PATH                   "${OGG_HOME}/bin:${PATH}:${JAVA_HOME}/bin"
ENV           LD_LIBRARY_PATH        "${JAVA_HOME}/jre/lib/amd64/server:${LD_LIBRARY_PATH}"
COPY          install-*.sh      /tmp/
COPY          ${INSTALLER}      /tmp/installer.zip
COPY          bin/              /usr/local/bin/

RUN           bash -c  /tmp/install-prerequisites.sh && \
              bash -c  /tmp/install-deployment.sh    && \
              rm   -fr /tmp/*   /etc/nginx

COPY          nginx/            /etc/nginx/

EXPOSE        80 443
VOLUME      [ "${OGG_DEPLOYMENT_HOME}", "${OGG_TEMPORARY_FILES}" ]
HEALTHCHECK   --start-period=90s --retries=1 \
        CMD [ "/usr/local/bin/healthcheck" ]
ENTRYPOINT  [ "/usr/local/bin/deployment-main.sh" ]
