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
  +-----------------------------------+  +-----------------------------------+
                    |                                      |
                    +--------------------------------------+
                                       |
                                     br-int
                                   10.6.8.254
                                       |
                                    HAProxy

  Contrail-A web UI:   https://<host>:8143
  OpenStack-A Horizon: http://<host>:8180
  Contrail-B web UI:   https://<host>:8243
  OpenStack-B Horizon: http://<host>:8280
```


# Resource
```
                  vCPU    memory(GB)    disk(GB)    OS
contrail-a          5        48             80      CentOS 7.5-1805
openstack-a         5        48             80      CentOS 7.5-1805
compute-a           3        16             40      CentOS 7.5-1805
contrail-b          5        48             80      CentOS 7.5-1805
openstack-b         5        48             80      CentOS 7.5-1805
compute-b           3        16             40      CentOS 7.5-1805
----------------------------------------------------------------
Total              26       224            400
```

