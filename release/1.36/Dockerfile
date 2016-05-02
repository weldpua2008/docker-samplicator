FROM ubuntu:14.04
MAINTAINER Valeriy Solovyov <weldpua2008@gmail.com>

RUN export SAMPLICATOR_VERSION=1.3.6 && \
    export SAMPLICATOR_BUIL_ROOT=/tmp/samplicator
    export URL=https://github.com/sleinen/samplicator/releases/download/v$SAMPLICATOR_VERSION/samplicator-$SAMPLICATOR_VERSION.tar.gz && \
    apt-get update && apt-get -y install tar build-essential && \
    mkdir -p $SAMPLICATOR_BUIL_ROOT && \
    tar xvf samplicator-$SAMPLICATOR_VERSION.tar.gz -C $SAMPLICATOR_BUIL_ROOT && \
    cd  $SAMPLICATOR_BUIL_ROOT && \
    ./configure && \
    make && \
    ln -s /$SAMPLICATOR_BUIL_ROOT/samplicate /usr/bin/samplicate
ENTRYPOINT ["samplicate"]
