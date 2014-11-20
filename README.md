# Consul + CoreOS - hassles

This is for running a [Consul](http://consul.io/) cluster on [CoreOS](http://coreos.com) using etcd and fleet to bootstrap it.

## Why?

While CoreOS already has etcd built in and it is very similar to Consul, it is missing some of the service discovery features offered by Consul. Namely the service catalog and service health checking. These things are nice to have.

This also allows you to run [Registrator](https://github.com/progrium/registrator) on CoreOS clusters and still get the Consul-backed extra features (such as health-check support).

## Usage

A service file is included, so you just need to submit it and run it on your CoreOS cluster:

```
fleetctl submit consul@.service
fleetctl start consul@1
fleetctl start consul@2
fleetctl start consul@3
...
```

## Implementation

It will use etcd and fleet to determine the size and configuration of your CoreOS cluster and spin up a Consul cluster on top of it. It runs Consul inside Docker containers (as nature intended) using [progrium's excellent Consul Docker image](https://github.com/progrium/docker-consul).

Note that as of now, it only supports running Consul on *every* CoreOS host in your cluster. This is because it looks at the output of `fleetctl list-machines` and tells Consul to expect that many hosts to connect before bootstrapping the cluster.
