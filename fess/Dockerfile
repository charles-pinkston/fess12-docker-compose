FROM openjdk:8-jre
LABEL maintainer "N2SM <support@n2sm.net>"

ARG FESS_VERSION

ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y \
      imagemagick \
      procps \
      unoconv \
      && \
    apt-get clean

RUN groupadd -g 1000 fess && \
    useradd -u 1000 fess -g fess

# RUN groupadd -g 1000 elasticsearch && \
#     groupadd -g 1001 fess && \
#     useradd -u 1000 elasticsearch -g elasticsearch && \
#     useradd -u 1001 fess -g fess

RUN set -x && \
    wget --progress=dot:mega https://github.com/codelibs/fess/releases/download/fess-${FESS_VERSION}/fess-${FESS_VERSION}.deb -O /tmp/fess-${FESS_VERSION}.deb && \
    dpkg -i /tmp/fess-${FESS_VERSION}.deb && \
    rm -rf /tmp/fess-${FESS_VERSION}.deb
RUN mkdir /opt/fess && \
    chown -R fess.fess /opt/fess && \
    sed -i -e 's#FESS_CLASSPATH="$FESS_CONF_PATH:$FESS_CLASSPATH"#FESS_CLASSPATH="/opt/fess:$FESS_CONF_PATH:$FESS_CLASSPATH"#g' /usr/share/fess/bin/fess
RUN echo "export FESS_APP_TYPE=$FESS_APP_TYPE" >>  /usr/share/fess/bin/fess.in.sh

USER fess
ENV PATH /usr/share/fess/bin:$PATH
WORKDIR /usr/share/fess

RUN set -ex && for fessdirs in logs; do \
        mkdir -p "$fessdirs"; \
        chown -R fess:fess "$fessdirs"; \
    done

COPY bin bin
COPY config/fess_config.properties /etc/fess

USER fess
RUN cp -r /etc/fess/* /usr/share/fess/app/WEB-INF/classes && \
    cp -r /etc/fess/* /usr/share/fess/lib/classes


CMD ["/bin/bash", "bin/fess-docker"]

EXPOSE 8080