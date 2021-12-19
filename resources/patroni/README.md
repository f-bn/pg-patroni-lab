### Patroni

#### Installation

* Add official PostgreSQL 14 RPM repository for Fedora (Patroni is included) :

  ```shell
  $ dnf in -y https://download.postgresql.org/pub/repos/yum/reporpms/F-35-x86_64/pgdg-fedora-repo-latest.noarch.rpm
  ```
 
* Install Patroni and required dependencies :

  ```shell
  $ dnf in -y patroni patroni-etcd postgresql14-server
  ```

  *NOTE*: `patroni-etcd` is installed here since I use etcd as DCS backend. Modify it depending of your DCS backend (Zookeeper, Kubernetes, Consul...)

* Apply the custom Patroni configuration on each node :

  ```shell
  $ mkdir -p /etc/patroni

  $ vi /etc/patroni/patroni.yml

    ---
    scope: patroni-lab
    name: patroni01
    ...
  ````  

* Enable and start the Patroni service sequentially on each node, the PostgreSQL instance should have been started by Patroni :

  ```shell
  $ systemctl enable --now patroni

  $ ps fauwx

  postgres   17943  0.0  1.4 555808 29900 ?        Ssl  16:47   0:00 /usr/bin/python3 /usr/bin/patroni /etc/patroni/patroni.yml
  postgres   17966  0.0  1.0 206664 21532 ?        S    16:47   0:00 /usr/pgsql-14/bin/postgres -D /var/lib/pgsql/14/data --config-file=/var/lib/pgsql/14/data/postgresql.conf --listen_addresses=* --port=5432 --clpostgres   17967  0.0  0.2  61368  4356 ?        Ss   16:47   0:00  \_ postgres: patroni-lab: logger
  postgres   17972  0.0  0.3 206868  7024 ?        Ss   16:47   0:00  \_ postgres: patroni-lab: checkpointer
  postgres   17973  0.0  0.2 206664  5212 ?        Ss   16:47   0:00  \_ postgres: patroni-lab: background writer
  postgres   17974  0.0  0.4 206664  9300 ?        Ss   16:47   0:00  \_ postgres: patroni-lab: walwriter
  postgres   17975  0.0  0.3 207196  7384 ?        Ss   16:47   0:00  \_ postgres: patroni-lab: autovacuum launcher
  postgres   17976  0.0  0.2  61392  4380 ?        Ss   16:47   0:00  \_ postgres: patroni-lab: stats collector
  postgres   17977  0.0  0.2 207172  5780 ?        Ss   16:47   0:00  \_ postgres: patroni-lab: logical replication launcher
  postgres   17979  0.0  0.7 209088 16020 ?        Ss   16:47   0:00  \_ postgres: patroni-lab: postgres postgres ::1(53464) idle
  postgres   18588  0.0  0.4 207816  9472 ?        Ss   16:53   0:00  \_ postgres: patroni-lab: walsender repuser 10.0.1.25(59662) streaming 0/5000148
  postgres   18640  0.0  0.4 207388  9096 ?        Ss   16:53   0:00  \_ postgres: patroni-lab: walsender repuser 10.0.1.26(59296) streaming 0/5000148
  ```

* Finally, ensure all Patroni nodes are available :

  ```shell
  $ patronictl -c /etc/patroni/patroni.yml list
  
  + Cluster: patroni-lab (7043440676997023270) ----+-----------+
  | Member    | Host      | Role    | State   | TL | Lag in MB |
  +-----------+-----------+---------+---------+----+-----------+
  | patroni01 | 10.0.1.24 | Leader  | running |  1 |           |
  | patroni02 | 10.0.1.25 | Replica | running |  1 |         0 |
  | patroni03 | 10.0.1.26 | Replica | running |  1 |         0 |
  ```
