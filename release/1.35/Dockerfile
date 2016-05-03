FROM ubuntu:14.04

MAINTAINER Valeriy Solovyov <weldpua2008@gmail.com>

RUN export SAMPLICATOR_VERSION=1.3.5 && \
    export SAMPLICATOR_BUIL_ROOT=/tmp/samplicator && \
    export SAMPLICATOR_URL=https://github.com/sleinen/samplicator/releases/download/v$SAMPLICATOR_VERSION/samplicator-$SAMPLICATOR_VERSION.tar.gz && \
    export SAMPLICATOR_FILE=samplicator-$SAMPLICATOR_VERSION.tar.gz &&\
    export buildDeps="tar build-essential" && \
    apt-get update && apt-get -y install $buildDeps && \
    apt-get install --no-install-recommends wget -y && \
    mkdir -p $SAMPLICATOR_BUIL_ROOT && \
    wget --no-check-certificate $SAMPLICATOR_URL -O $SAMPLICATOR_FILE &&\
    tar xvf $SAMPLICATOR_FILE -C $SAMPLICATOR_BUIL_ROOT --strip-components=1 && \
    cd  $SAMPLICATOR_BUIL_ROOT && \
    ./configure && \
    make && \
    make install && \
    rm -r /var/lib/apt/lists/* && \
    cd / && \
    rm -rf $SAMPLICATOR_BUIL_ROOT

#ln -s /$SAMPLICATOR_BUIL_ROOT/samplicate /usr/bin/samplicate

ENTRYPOINT ["samplicate"]
