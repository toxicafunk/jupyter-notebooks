FROM quay.io/jupyteronopenshift/s2i-minimal-notebook-py36:2.5.1

USER root

COPY . /tmp/src

RUN rm -rf /tmp/src/.git* && \
    chown -R 1001 /tmp/src && \
    chgrp -R 0 /tmp/src && \
    chmod -R g+w /tmp/src && \
    rm -rf /tmp/scripts && \
    mv /tmp/src/.s2i/bin /tmp/scripts

RUN yum -y install java-1.8.0-openjdk-devel

ENV XDG_CACHE_HOME=/opt/app-root/src/.cache
ENV SCALA_VERSION=2.12.10
ENV ALMOND_VERSION=0.9.1

USER 1001

RUN curl -Lo coursier https://git.io/coursier-cli && \
    chmod +x coursier && \
    ./coursier bootstrap \
      -r jitpack \
      -i user -I user:sh.almond:scala-kernel-api_$SCALA_VERSION:$ALMOND_VERSION \
      sh.almond:scala-kernel_$SCALA_VERSION:$ALMOND_VERSION \
    -o almond

RUN ./almond --install

#RUN /tmp/scripts/assemble

CMD [ "/opt/app-root/builder/run" ]
