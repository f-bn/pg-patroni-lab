### HAProxy Load Balancer with Keepalived VIP

#### HAProxy installation

* Firstly, install the HAProxy package :

  ```shell
  $ dnf in -y haproxy
  ```

* Then, apply some sysctls to ensure HAProxy :

  ```
  $ vi /etc/sysctl.d/99-proxy.conf

  net.ipv4.ip_nonlocal_bind = 1
  net.ipv6.ip_nonlocal_bind = 1
  net.ipv4.conf.all.arp_announce = 2
  net.ipv4.conf.all.arp_ignore = 1
  net.ipv4.ip_forward = 1

  $ sysctl -p --system
  ```  

  *NOTE*: this only works on bare-metal or in a virtual-machine, this won't work in a container (use options provided by the container runtime to apply these sysctl).

* Apply the configuration on each HAProxy node

  ```shell
  $ vi /etc/haproxy/haproxy.conf
  ...
  ```

* Enable and start the HAProxy service

  ```shell
  $ systemctl enable --now haproxy
  ```

#### Keepalived

* Firstly, install Keepalived package :

  ```shell
  $ dnf in -y keepalived
  ```

* Then, apply the Keepalived configuration on each node (there is one configuration file for the `MASTER` node and another one for the `BACKUP` node)

  ```shell
  $ vi /etc/keepalived/keepalived.conf
  ...
  ```

* Finally, enable and start the Keepalived service :

  ```shell
  $ systemctl enable --now keepalived
  ```

* Check if your VIP is mounted on your main NIC :

  ```shell
  $ ip a sh eth0

    83: eth0@if84: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 00:16:3e:e3:1f:60 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.0.1.22/24 metric 1024 brd 10.0.1.255 scope global dynamic eth0
       valid_lft 3376sec preferred_lft 3376sec
    inet 10.0.1.20/32 scope global eth0 <=========================
       valid_lft forever preferred_lft forever
    inet6 fe80::216:3eff:fee3:1f60/64 scope link
       valid_lft forever preferred_lft forever
  ```

