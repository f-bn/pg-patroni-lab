<p><img src="https://icon-library.com/images/postgresql-icon/postgresql-icon-20.jpg" alt="pg-logo" title="pg" align="top" height=220 /></p>

Configurations to deploy HA PostgreSQL cluster using Zalando Patroni for training purpose.

## Cluster design

### Overview

*TBD*

### Components

  - **PostgreSQL version** : `14.1`
  - **Patroni version** : `2.1.2`
  - **DCS** : etcd `3.5.0`
  - **OS** : Fedora 35

### Nodes description

* **PostgreSQL cluster**
  - 3x **PostgreSQL 14.1** instances in replication

* **DCS cluster**
  - 3x **etcd** nodes in HA cluster

## References

- **PostgreSQL** : https://www.postgresql.org/
- **Patroni (Zalando)** : https://github.com/zalando/patroni
- **etcd** : https://etcd.io/
- **Zookeeper** : https://zookeeper.apache.org/
