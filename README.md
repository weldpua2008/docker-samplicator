[![Docker Pulls](https://img.shields.io/docker/pulls/weldpua2008/docker-samplicator.svg)](https://hub.docker.com/r/weldpua2008/docker-samplicator/)
[![Docker Stars](https://img.shields.io/docker/stars/weldpua2008/samplicator.svg)](https://hub.docker.com/r/weldpua2008/samplicator/)
[![dockeri.co](http://dockeri.co/image/weldpua2008/samplicator)](https://hub.docker.com/r/weldpua2008/samplicator/)


docker-samplicator
===================
These are base docker images that include samplicator.  

## Description
A generic UDP proxy/duplicator.  Forwards UDP requests to defined hosts.  I am currently using this as an SNMP trap proxy.  For a working example, run:

    docker run -d --name trap_sink elcolio/net-snmp
    export TRAPSINK=`docker inspect -f '{{ .NetworkSettings.IPAddress }}' trap_sink`
    docker run -d --name trap_dest elcolio/net-snmp
    export TRAPDEST=`docker inspect -f '{{ .NetworkSettings.IPAddress }}' trap_dest`
    docker run -d --name trap_proxy weldpua2008/samplicator -d 3 -p 162 -S $TRAPSINK/162 $TRAPDEST/162
    export TRAP_PROXY=`docker inspect -f '{{ .NetworkSettings.IPAddress }}' trap_proxy`
    docker run --rm=true elcolio/net-snmp snmptrap -v 1 -c public $TRAP_PROXY .1.3.6.1.6.3 "" 0 0 coldStart.0

This will start 2 trap receivers (trap_sink is a local logging receiver and trap_dest is the real receiver) and send a coldStart trap to the proxy.  Since debugging is turned on, you'll be able to see the packet come through the proxy (using docker logs trap_proxy).  Then you can examine /trap.log on the receivers to see the content (using [docker-enter][1] it would be docker-enter trap_sink cat trap.log)

## Project Source
https://github.com/sleinen/samplicator

## Usage
To see this help on the CLI:  **docker run --rm=true elcolio/samplicator -h**

    This is samplicate, version 1.3.7-beta6.
    Usage: samplicate [option...] receiver...

    Supported options:

      -p <port>                UDP port to accept flows on (default 2000)
      -s <address>             Interface address to accept flows on (default any)
      -d <level>               debug level
      -b <size>                set socket buffer size (default 65536)
      -n			   don't compute UDP checksum (leave at 0)
      -S                       maintain (spoof) source addresses
      -x <delay>               transmit delay in microseconds
      -c configfile            specify a config file to read
      -f                       fork program into background
      -V                       print program version and exit
      -h                       print this usage message and exit

    Specifying receivers:

      A.B.C.D[/port[/freq][,ttl]]...
    where:
      A.B.C.D                  is the receiver's IP address
      port                     is the UDP port to send to (default 2000)
      freq                     is the sampling rate (default 1)
      ttl                      is the sending packets TTL value (default 64)

    Config file format:

      a.b.c.d[/e.f.g.h]: receiver ...
    where:
      a.b.c.d                  is the senders IP address
      e.f.g.h                  is a mask to apply to the sender (default 255.255.255.255)
      receiver                 see above.

    Receivers specified on the command line will get all packets, those
    specified in the config-file will get only packets with a matching source.


Requirements
=====================================

* docker

Obtaining these Images from DockerHub
=====================================

This content on DockerHub lives at https://hub.docker.com/r/weldpua2008/docker-samplicator/

* This images on DockerHub will be updated periodically.

Disclaimer
=============================

This is currently a work in progress, I am not responsible and shall not
be held responsible in any manner if this tool causes loss of data, hardware
faults, act of gods, invocation of old or ancient ones, elder gods and other
horrors from the depths.

Questions?
==========

If you'd like to talk about Ansible+Docker, stop by the .


  [1]: https://github.com/jpetazzo/nsenter