# Contrail POC
# Cluster - Contrail Fabric Management Remote

# Topology

```
                          br-r1                                 br-r2
                        10.6.11.0/24                          10.6.12.0/24
                            |                                     |
  +---------------------------------+  +---------------------------------+
  | contrail   10.6.8.1  10.6.11.1  |  | csn-b      10.6.8.13  10.6.12.3 |
  | openstack  10.6.8.2  10.6.11.2  |  | compute-b  10.6.8.14  10.6.12.4 |
  | csn-a      10.6.8.3  10.6.11.3  |  +---------------------------------+
  | compute-a  10.6.8.4  10.6.11.4  |                  |
  | command    10.6.8.9  10.6.11.9  |                  |
  +---------------------------------+                  |
                  |                                    |
                  +------------------------------------+
                                     |
                                   br-int
                                 10.6.8.254
                                     |
                                  HAProxy

  Contrail Command:  https://<host>:9191
  Contrail web UI:   https://<host>:8143
  OpenStack Horizon: http://<host>:8180
```


# Resource
```
                  vCPU    memory(GB)    disk(GB)    OS
command             2        32            80      CentOS 7.5-1805
openstack           5        48            100     CentOS 7.5-1805
contrail            5        48            100     CentOS 7.5-1805
csn-a               1         8            40      CentOS 7.5-1805
csn-b               1         8            40      CentOS 7.5-1805
compute             2        16            40      CentOS 7.5-1805
compute             2        16            40      CentOS 7.5-1805
----------------------------------------------------------------
Total              18       176            440
```

