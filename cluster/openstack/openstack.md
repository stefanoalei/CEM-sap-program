# Contrail POC
# Cluster - OpenStack

# Topology

```
                            br-r1  10.6.11.0/24
                              |
  +-----------------------------------+
  | contrail    10.6.8.1   10.6.11.1  |
  | openstack   10.6.8.2   10.6.11.2  |
  | compute-1   10.6.8.3   10.6.11.3  |
  | compute-2   10.6.8.4   10.6.11.4  |
  +-----------------------------------+
                    |
                  br-int
                10.6.8.254
                    |
                 HAProxy

  Contrail web UI:   https://<host>:8143
  OpenStack Horizon: http://<host>
```


# Resource
```
                  vCPU    memory(GB)    disk(GB)    OS
openstack           5        64            100      CentOS 7.5-1805
contrail            5        64            100      CentOS 7.5-1805
compute-1           4        32             80      CentOS 7.5-1805
compute-2           4        32             80      CentOS 7.5-1805
----------------------------------------------------------------
Total              18       192            360
```

