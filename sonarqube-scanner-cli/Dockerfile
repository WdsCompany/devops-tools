FROM node:20

ARG SONAR_SCANNER_HOME=/opt/sonar-scanner
ARG SONAR_SCANNER_VERSION=6.1.0.4477
ENV HOME=/tmp \
    XDG_CONFIG_HOME=/tmp \
    SONAR_SCANNER_HOME=${SONAR_SCANNER_HOME} \
    SCANNER_BINARIES=https://binaries.sonarsource.com/Distribution/sonar-scanner-cli
ENV SCANNER_ZIP_URL="${SCANNER_BINARIES}/sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux-x64.zip"

WORKDIR /opt

RUN apt-get update && apt-get install -y openjdk-17-jre

RUN set -eux; \
    apt update  || apt install -y gnupg unzip wget; \
    wget -U "scannercli" -q -O /opt/sonar-scanner-cli.zip ${SCANNER_ZIP_URL}; \
    unzip sonar-scanner-cli.zip; \
    mv sonar-scanner-${SONAR_SCANNER_VERSION}-linux-x64 ${SONAR_SCANNER_HOME};

ENV JAVA_HOME=/usr/lib/jvm/java-1.17.0-openjdk-amd64/
ENV PATH=$PATH:$JAVA_HOME/bin
ENV PATH=${SONAR_SCANNER_HOME}/bin:${PATH}
ENV SONAR_SCANNER_OPTS="-Djavax.net.ssl.trustStore=/usr/lib/jvm/java-1.17.0-openjdk-amd64/lib/security/cacerts -Djavax.net.ssl.trustStorePassword=changeit -Dsonar.scanner.skipJreProvisioning=true -Dsonar.scanner.javaExePath=/usr/lib/jvm/java-1.17.0-openjdk-amd64/bin"

WORKDIR /app

ADD sonar-project.properties sonar-project.properties
ADD server.crt /usr/local/share/ca-certificates/server.crt
ADD rootCA.crt /usr/local/share/ca-certificates/rootCA.crt

RUN /usr/sbin/update-ca-certificates

RUN keytool -trustcacerts -storepass changeit -importcert -noprompt -alias server.crt -file /usr/local/share/ca-certificates/server.crt
RUN keytool -trustcacerts -storepass changeit -importcert -noprompt -alias rootCA.crt -file /usr/local/share/ca-certificates/rootCA.crt
