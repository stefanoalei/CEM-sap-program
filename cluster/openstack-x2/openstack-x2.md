# Contrail POC
# Cluster - OpenStack x2

# Topology

```
                            br-r1                                   br-r2
                          10.6.11.0/24                            10.6.21.0/24
                              |                                       |
  +-----------------------------------+  +-----------------------------------+
  | contrail-a   10.6.8.1  10.6.11.1  |  | contrail-b   10.6.8.11  10.6.21.1 |
  | openstack-a  10.6.8.2  10.6.11.2  |  | openstack-b  10.6.8.12  10.6.21.2 |
  | compute-a    10.6.8.3  10.6.11.3  |  | compute-b    10.6.8.13  10.6.21.3 |
  | command-a    10.6.8.9  10.6.11.9  |  | command-b    10.6.8.19  10.6.21.9 |
  +-----------------------------------+  +-----------------------------------+
                    |                                      |
                    +--------------------------------------+
                                       |
                                     br-int
                                   10.6.8.254
                                       |
                                    HAProxy

  Contrail-A web UI:   https://<host>:8143
  Contrail-A Command:  https://<host>:9191
  OpenStack-A Horizon: http://<host>:8180
  Contrail-B web UI:   https://<host>:8243
  Contrail-B Command:  https://<host>:9291
  OpenStack-B Horizon: http://<host>:8280
```


# Resource
```
                  vCPU    memory(GB)    disk(GB)    OS
contrail-a          4        48             80      CentOS 7.5-1805
openstack-a         4        48             80      CentOS 7.5-1805
command-a           2        16             40      CentOS 7.5-1805
compute-a           3         8             40      CentOS 7.5-1805
contrail-b          4        48             80      CentOS 7.5-1805
openstack-b         4        48             80      CentOS 7.5-1805
command-b           2        16             40      CentOS 7.5-1805
compute-b           3         8             40      CentOS 7.5-1805
----------------------------------------------------------------
Total              26       240            480
```

