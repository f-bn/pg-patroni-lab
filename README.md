<p><img src="https://icon-library.com/images/postgresql-icon/postgresql-icon-20.jpg" alt="pg-logo" title="pg" align="top" height=220 /></p>

*Patroni is a template for you to create your own customized, high-availability solution using Python and - for maximum accessibility - a distributed configuration store like ZooKeeper, etcd, Consul or Kubernetes. Database engineers, DBAs, DevOps engineers, and SREs who are looking to quickly deploy HA PostgreSQL in the datacenter-or anywhere else-will hopefully find it useful*

## General informations

This repository contains configurations for deploying a PostgreSQL HA cluster using Zalando Patroni (for training purpose).

### Overview

![Patroni cluster](docs/patroni-11012022-1.png)

### Components

  - **PostgreSQL version** : `14.1`
  - **Patroni version** : `2.1.2`
  - **DCS (Distributed Configuration Store)** : etcd `3.5.0`
  - **OS** : Fedora 35

### Nodes description

The cluster is composed of two main "sub" clusters :

* **Front Load Balancers**
  - 2x **HAProxy LB** in Active/Passive mode + Keepalived VIP

* **PostgreSQL cluster**
  - 1x **Primary** PostgreSQL instance
  - 2x **Standby** PostgreSQL instances in streaming replication (managed by Patroni)

* **DCS cluster**
  - 3x **etcd** nodes in HA cluster

## Deployment and administration

* **HAProxy Load Balancer**
  - [HAProxy](resources/front-lb/README.md#haproxy-installation)
  - [Keepalived VIP](resources/front-lb/README.md#keepalived)

* **DCS cluster bootstrap**
  - [etcd](resources/dcs/etcd/README.md)

* **Patroni**
  - [Patroni installation](resources/patroni/README.md#installation)

## References

- **PostgreSQL** : https://www.postgresql.org/
- **Patroni (Zalando)** : https://github.com/zalando/patroni
- **pgBouncer** : https://www.pgbouncer.org/
- **etcd** : https://etcd.io/
- **Apache Zookeeper** : https://zookeeper.apache.org/
- **HAProxy** : https://www.haproxy.com/
