# Contrail Virtual Fabric
# Cluster - OpenStack HA

# Topology

```
                              br-r1  10.6.11.0/24
                                |
  +-------------------------------------+
  | contrail-1    10.6.8.1   10.6.11.1  |
  | contrail-2    10.6.8.2   10.6.11.2  |
  | contrail-3    10.6.8.3   10.6.11.3  |
  | openstack-1   10.6.8.4   10.6.11.4  |
  | openstack-2   10.6.8.5   10.6.11.5  |
  | openstack-3   10.6.8.6   10.6.11.6  |
  | compute-1     10.6.8.7   10.6.11.7  |
  | compute-2     10.6.8.8   10.6.11.8  |
  +-------------------------------------+
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
openstack-1         4        32            100      CentOS 7.5-1805
openstack-2         4        32            100      CentOS 7.5-1805
openstack-3         4        32            100      CentOS 7.5-1805
contrail-1          4        32            100      CentOS 7.5-1805
contrail-2          4        32            100      CentOS 7.5-1805
contrail-3          4        32            100      CentOS 7.5-1805
compute-1           3        16             80      CentOS 7.5-1805
compute-2           3        16             80      CentOS 7.5-1805
----------------------------------------------------------------
Total              30       224           760
```

