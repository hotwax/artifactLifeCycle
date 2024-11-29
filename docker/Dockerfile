FROM ubuntu:18.04
LABEL maintainer="HotWax Commerce <sysadmin@hotwaxsystems.com>"

# Install necessary packages in a single RUN command to reduce image layers
RUN apt-get update && \
    apt-get -y install openjdk-11-jdk ca-certificates-java git vim screen bash && \
    apt-get clean

ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
ENV PATH $JAVA_HOME/bin:$PATH

WORKDIR /

RUN git clone --depth 1 -b "main" https://github.com/hotwax/moqui-framework.git /moqui-framework
WORKDIR /moqui-framework
RUN git clone --depth 1 -b "main" https://github.com/hotwax/moqui-runtime.git runtime
WORKDIR /moqui-framework/runtime/component
RUN git clone --depth 1 -b "main" https://github.com/hotwax/artifactLifeCycle.git

WORKDIR /moqui-framework

RUN echo "org.gradle.daemon=false" | tee -a gradle.properties
RUN git apply runtime/component/artifactLifeCycle/patches/ElasticDatasourceFactory.patch

RUN ./gradlew addRuntime
RUN mkdir -p /moqui-deploy && \
    cd /moqui-deploy && \
    jar -xvf /moqui-framework/moqui-plus-runtime.war

ENV JAVA_OPTS="-Xms512m -Xmx4096m"
ENV CONF_FILE="/moqui-framework/runtime/conf/MoquiProductionConf.xml"
ENV SLEEP="sleep 2"

ENV Moqui_HOST=""
ENV Moqui_DB_HOST=""
ENV Moqui_DB_USER=""
ENV Moqui_DB_PASSWORD=""
ENV Moqui_DB_NAME=""

ENV ELASTICSEARCH_HOST=""
ENV ELASTICSEARCH_USER=""
ENV ELASTICSEARCH_PASSWORD=""

ENV TIME_ZONE=""
ENV Moqui_configuration_DB_Name=""
ENV SCHEDULED_JOB_CHECK_TIME="60"

WORKDIR /moqui-deploy
COPY docker/entrypoint.sh .
RUN chmod +x entrypoint.sh

# Expose the application port
EXPOSE 8080

# Define the entrypoint
ENTRYPOINT ./entrypoint.sh && bash