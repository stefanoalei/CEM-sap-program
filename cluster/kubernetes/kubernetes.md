# Contrail POC
# Cluster - Kubernetes

# Topology

```
                         br-r1  10.6.11.0/24
                           |
  +--------------------------------+
  | master   10.6.8.1   10.6.11.1  |
  | node-1   10.6.8.2   10.6.11.2  |
  | node-2   10.6.8.3   10.6.11.3  |
  +--------------------------------+
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
master           6        64            100      CentOS 7.5-1805
node-1           4        16             80      CentOS 7.5-1805
node-2           4        16             80      CentOS 7.5-1805
----------------------------------------------------------------
Total           14        96            260
```

