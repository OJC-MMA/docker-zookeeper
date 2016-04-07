FROM java:8-jre-alpine
MAINTAINER smizy

ENV ZOOKEEPER_VERSION     3.4.8
ENV ZOOKEEPER_HOME        /usr/local/zookeeper-${ZOOKEEPER_VERSION}
ENV ZOOKEEPER_DATA_DIR    /zookeeper
ENV ZOOKEEPER_DATALOG_DIR /var/log/zookeeper
ENV ZOOKEEPER_CONF_DIR    ${ZOOKEEPER_HOME}/conf
ENV PATH                  $PATH:${ZOOKEEPER_HOME}/bin

RUN set -x \
    && apk --no-cache add \
        bash \
        su-exec \ 
    && mirror_url=$( \
        wget -q -O - http://www.apache.org/dyn/closer.cgi/zookeeper/ \
        | sed -n 's#.*href="\(http://ftp.[^"]*\)".*#\1#p' \
        | head -n 1 \
    ) \   
    && wget -q -O - ${mirror_url}/zookeeper-${ZOOKEEPER_VERSION}/zookeeper-${ZOOKEEPER_VERSION}.tar.gz \
        | tar -xzf - -C /usr/local \
    ## user/dir/permmsion
    && adduser -D  -g '' -s /sbin/nologin -u 1000 docker \
    && adduser -D  -g '' -s /sbin/nologin zookeeper \
    && mkdir -p \
        ${ZOOKEEPER_DATA_DIR} \
        ${ZOOKEEPER_DATALOG_DIR} \
    && chown -R zookeeper:zookeeper \
        ${ZOOKEEPER_HOME} \
        ${ZOOKEEPER_DATA_DIR} \
        ${ZOOKEEPER_DATALOG_DIR} 

COPY entrypoint.sh /usr/local/bin/ 
COPY zoo.cfg       ${ZOOKEEPER_CONF_DIR}/
 
VOLUME ["${ZOOKEEPER_DATA_DIR}", "${ZOOKEEPER_DATALOG_DIR}"]

WORKDIR ${ZOOKEEPER_HOME}

EXPOSE 2181 2888 3888

ENTRYPOINT ["entrypoint.sh"]
CMD ["-server", "1", "1" ]