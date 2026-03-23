ARG build_image="docker.io/alpine:3"
ARG runtime_image="docker.io/eclipse-temurin:25-jdk"

# ======================
# BUILDER
# ======================

FROM $build_image AS builder
ARG hawtio_app_version="2.17.7"

RUN set -ex \
  && apk --no-cache add curl

RUN set -ex \
  && mkdir -p /exports \
  && curl "https://repo1.maven.org/maven2/io/hawt/hawtio-app/${hawtio_app_version}/hawtio-app-${hawtio_app_version}.jar" -o /exports/hawtio.jar

# ======================
# RUNTIME
# ======================

FROM $runtime_image
COPY --from=builder ["/exports/hawtio.jar", "/opt/hawtio/hawtio.jar"]

COPY <<HEREDOC /usr/local/bin/docker-entrypoint.sh
#!/bin/sh
set -ex
printf "[$(date)] Starting Hawtio .. \n"
exec java $JAVA_OPTS -jar /opt/hawtio/hawtio.jar $@
HEREDOC
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

ENV JAVA_OPTS="-Dhawtio.proxyAllowlist=* -Dhawtio.remote.name=activemq -Dhawtio.remote.host=activemq -Dhawtio.remote.port=8778 -Dhawtio.remote.path=jolokia"
ENV JAVA_TOOL_OPTIONS="-XX:+ExitOnOutOfMemoryError -XX:InitialRAMPercentage=50.0 -XX:MaxRAMPercentage=75.0 -XX:+UseG1GC -XX:+UseStringDeduplication -XX:MaxGCPauseMillis=500 -XX:+PerfDisableSharedMem -Xss512k -Xshare:on -Xlog:gc*:stdout:uptime,level,tags"
EXPOSE 8080
ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]
