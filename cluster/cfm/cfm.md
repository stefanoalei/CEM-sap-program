# Contrail POC
# Cluster - Contrail Fabric Management

# Topology

```
                            br-r1  10.6.11.0/24
                              |
  +-----------------------------------+
  | contrail    10.6.8.1   10.6.11.1  |
  | openstack   10.6.8.2   10.6.11.2  |
  | csn         10.6.8.3   10.6.11.3  |
  | compute     10.6.8.4   10.6.11.4  |
  | command     10.6.8.10  10.6.11.10 |
  +-----------------------------------+
                    |
                  br-int
                10.6.8.254
                    |
                 HAProxy

  Contrail web UI:   https://<host>:8143
  Contrail Command:  https://<host>:9091
  OpenStack Horizon: http://<host>
```


# Resource
```
                  vCPU    memory(GB)    disk(GB)    OS
command             2        32             80      CentOS 7.5-1805
openstack           4        64            100      CentOS 7.5-1805
contrail            4        64            100      CentOS 7.5-1805
csn                 1        16             40      CentOS 7.5-1805
compute             4        32             60      CentOS 7.5-1805
----------------------------------------------------------------
Total              15       208            380
```

