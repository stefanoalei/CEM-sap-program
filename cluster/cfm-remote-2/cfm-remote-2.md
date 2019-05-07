# Contrail POC
# Cluster - Contrail Fabric Management Remote Site 2

# Topology

```
                           br-r3                                  br-r4
                         10.6.13.0/24                           10.6.14.0/24
                             |                                      |
  +----------------------------------+  +----------------------------------+
  | contrail   10.6.8.51  10.6.13.1  |  | csn-b      10.6.8.55   10.6.14.5 |
  | openstack  10.6.8.52  10.6.13.2  |  | compute-b  10.6.8.56   10.6.14.6 |
  | csn-a      10.6.8.53  10.6.13.3  |  +----------------------------------+
  | compute-a  10.6.8.54  10.6.13.4  |                  |
  +----------------------------------+                  |
                  |                                     |
                  +-------------------------------------+
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
csn-a               1         8            40      CentOS 7.5-1805
csn-b               1         8            40      CentOS 7.5-1805
compute             2        16            40      CentOS 7.5-1805
compute             2        16            40      CentOS 7.5-1805
----------------------------------------------------------------
Total              17       160            360
```

