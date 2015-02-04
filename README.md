nsq-docker
==========

Provides automated builds for NSQ using binaries directly from github.


Usage
=====

Start up a single host NSQ cluster only accessible from the docker host:

    docker run -d --name nsqlookupd virtuald/nsq:nsqlookupd-0.3.0 -broadcast-address '$HOSTNAME'
    docker run -d --name nsqd --link nsqlookupd:nsqlookupd virtuald/nsq:nsqd-0.3.0 -broadcast-address '$HOSTNAME'
    docker run -d --name nsqadmin --link nsqlookupd:nsqlookupd virtuald/nsq:nsqadmin-0.3.0

Note that in this configuration, this cluster cannot be talked to from the
outside world. If you want to do that, then you need to use -p to publsh all
the ports, and set the broadcast address to the docker host address.

That looks like this:

	docker run -d --name nsqlookupd -p 4160:4160 -p 4161:4161 virtuald/nsq:nsqlookupd-0.3.0 -broadcast-address '$HOSTNAME'
	docker run -d --name nsqd -p 4150:4150 -p 4151:4151 --link nsqlookupd:nsqlookupd virtuald/nsq:nsqd-0.3.0 -broadcast-address '$HOSTNAME'
	docker run -d --name nsqadmin -p 4171:4171 --link nsqlookupd:nsqlookupd virtuald/nsq:nsqadmin-0.3.0

