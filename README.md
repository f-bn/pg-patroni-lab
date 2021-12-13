<p><img src="https://icon-library.com/images/postgresql-icon/postgresql-icon-20.jpg" alt="pg-logo" title="pg" align="top" height=220 /></p>

Configurations to deploy HA PostgreSQL cluster using Patroni for training purpose.

## Cluster design

### Overview

*TBD*

### Components

  - **PostgreSQL version** : 14
  - **Patroni version** : *TBD*
  - **DCS** : Apache Zookeeper
  - **OS** : Fedora 35

### Nodes description

* **PostgreSQL cluster**
  - 3x **PostgreSQL** instances in replication

* **DCS cluster**
  - 3x **Zookeeper** nodes

## References

- **PostgreSQL** : https://www.postgresql.org/
- **Patroni (Zalando)** : https://github.com/zalando/patroni
- **Zookeeper** : https://zookeeper.apache.org/
