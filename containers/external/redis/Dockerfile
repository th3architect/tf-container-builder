ARG CONTRAIL_REGISTRY
ARG CONTRAIL_CONTAINER_TAG
FROM ${CONTRAIL_REGISTRY}/contrail-general-base:${CONTRAIL_CONTAINER_TAG}

ARG CONTAINER_NAME
ENV CONTRAIL_VERSION=$CONTRAIL_CONTAINER_TAG \
    SERVICE_NAME=redis
LABEL name=$CONTAINER_NAME \
      summary="Contrail Redis" \
      description="Contrail Redis server supports query engine." \
      $VENDOR_DOMAIN".service"=$SERVICE_NAME \
      $VENDOR_DOMAIN".container.name"=$CONTAINER_NAME

ARG REDIS_RPM_NAME=redis40u-4.0.14-2.el7.ius.x86_64
ARG REDIS_CACHE=https://github.com/tungstenfabric/tf-third-party-cache/blob/master/redis

COPY *.sh hostname_to_ip /

RUN set -eux && for item in `ls /__*` ; do mv $item /${item:3} ; done && \
    if ! yum repolist | grep -q epel ; then \
        yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm || exit 1 ; \
    fi && \
    yum install perl -y && \
    wget -nv --tries=3 -c -O ${REDIS_RPM_NAME}.rpm ${REDIS_CACHE}/${REDIS_RPM_NAME}.rpm?raw=true && \
    yum install ${REDIS_RPM_NAME}.rpm -y && \
    usermod -a -G $CONTRAIL_GID redis && \
    rm -rf ${REDIS_RPM_NAME}.rpm && \
    yum clean all -y && rm -rf /var/cache/yum

RUN mkdir /data && chown redis:redis /data
VOLUME /data
WORKDIR /data

ENTRYPOINT ["/contrail-entrypoint.sh"]

EXPOSE 6379

CMD ["redis-server"]
