ARG CONTRAIL_REGISTRY
ARG CONTRAIL_CONTAINER_TAG
FROM ${CONTRAIL_REGISTRY}/contrail-base:${CONTRAIL_CONTAINER_TAG}

ARG CONTAINER_NAME
LABEL name=$CONTAINER_NAME \
      summary="Container with code of contrail-status tool" \
      description="Display a list of all components of a Contrail server node and report their current status of active or inactive." \
      $VENDOR_DOMAIN".container.name"=$CONTAINER_NAME

RUN yum install -y python2-docker PyYAML podman contrail-nodemgr && \
    yum clean all -y && \
    rm -rf /var/cache/yum && \
    pip2 install --no-compile grpcio==1.29.0 protobuf==3.12.1

COPY contrail-status.py /root
COPY entrypoint.sh /

ENTRYPOINT ["/entrypoint.sh"]

ENV PS1="\033[1m($CONTAINER_NAME)\033[m\017[$(id -un)@$(hostname -s) $(pwd)]$ "
