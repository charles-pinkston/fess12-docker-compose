FROM docker.elastic.co/elasticsearch/elasticsearch:6.2.4

RUN set -ex && for esdirs in config data logs; do \
        mkdir -p "$esdirs"; \
        chown -R elasticsearch:elasticsearch "$esdirs"; \
    done

RUN \
  mkdir "/var/lib/elasticsearch" \
  && chown -R elasticsearch:elasticsearch "/var/lib/elasticsearch"

USER elasticsearch

RUN /usr/share/elasticsearch/bin/elasticsearch-plugin install org.codelibs:elasticsearch-analysis-fess:6.2.1 && \
    /usr/share/elasticsearch/bin/elasticsearch-plugin install org.codelibs:elasticsearch-analysis-ja:6.2.1 && \
    /usr/share/elasticsearch/bin/elasticsearch-plugin install org.codelibs:elasticsearch-analysis-synonym:6.2.1 && \
    /usr/share/elasticsearch/bin/elasticsearch-plugin install org.codelibs:elasticsearch-configsync:6.2.2 && \
    /usr/share/elasticsearch/bin/elasticsearch-plugin install org.codelibs:elasticsearch-dataformat:6.2.3 && \
    /usr/share/elasticsearch/bin/elasticsearch-plugin install org.codelibs:elasticsearch-langfield:6.2.1 && \
    /usr/share/elasticsearch/bin/elasticsearch-plugin install org.codelibs:elasticsearch-analysis-kuromoji-neologd:6.2.1 && \
    /usr/share/elasticsearch/bin/elasticsearch-plugin install org.codelibs:elasticsearch-minhash:6.2.1

COPY config config
COPY bin bin

USER root
RUN chown -R elasticsearch:elasticsearch config bin && \
    chmod 0750 bin/es-docker

USER elasticsearch
CMD ["/bin/bash", "/usr/share/elasticsearch/bin/elasticsearch"]

EXPOSE 9200 9300