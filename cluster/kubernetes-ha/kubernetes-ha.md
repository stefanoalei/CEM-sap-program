# Contrail POC
# Cluster - Kubernetes HA

# Topology

```
                           br-r1  10.6.11.0/24
                             |
  +----------------------------------+
  | master-1   10.6.8.1   10.6.11.1  |
  | master-2   10.6.8.2   10.6.11.2  |
  | master-3   10.6.8.3   10.6.11.3  |
  | node-1     10.6.8.4   10.6.11.4  |
  | node-2     10.6.8.5   10.6.11.5  |
  | node-3     10.6.8.6   10.6.11.6  |
  | node-4     10.6.8.7   10.6.11.7  |
  +----------------------------------+
                   |
                 br-int
               10.6.8.254
                   |
                HAProxy

  Contrail web UI:   https://<host>:8143
```


# Resource
```
                vCPU    memory(GB)    disk(GB)    OS
master-1         5        48            100      CentOS 7.5-1805
master-2         5        48            100      CentOS 7.5-1805
master-3         5        48            100      CentOS 7.5-1805
node-1           3        16             60      CentOS 7.5-1805
node-2           3        16             60      CentOS 7.5-1805
node-3           3        16             60      CentOS 7.5-1805
node-4           3        16             60      CentOS 7.5-1805
----------------------------------------------------------------
Total           27       208            540
```

