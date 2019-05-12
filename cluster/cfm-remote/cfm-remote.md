# Contrail POC
# Cluster - Contrail Fabric Management Remote

# Topology

```
                          br-r1                                 br-r2
                        10.6.11.0/24                          10.6.12.0/24
                            |                                     |
  +---------------------------------+  +---------------------------------+
  | contrail   10.6.8.1  10.6.11.1  |  | csn-2      10.6.8.5   10.6.12.5 |
  | openstack  10.6.8.2  10.6.11.2  |  | compute-2  10.6.8.6   10.6.12.6 |
  | csn-1      10.6.8.3  10.6.11.3  |  +---------------------------------+
  | compute-1  10.6.8.4  10.6.11.4  |                  |
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
contrail            6        64            100     CentOS 7.5-1805
openstack           5        48            100     CentOS 7.5-1805
csn-1               1         8            40      CentOS 7.5-1805
csn-2               1         8            40      CentOS 7.5-1805
compute-1           2        16            40      CentOS 7.5-1805
compute-2           2        16            40      CentOS 7.5-1805
----------------------------------------------------------------
Total              17       160            360
```

