# Contrail POC
# Cluster - CFM and Kubernetes

# Topology

```
                         br-r1  10.6.11.0/24
                           |
  +--------------------------------+
  | master   10.6.8.1   10.6.11.1  |
  | csn      10.6.8.2   10.6.11.2  |
  | node-1   10.6.8.3   10.6.11.3  |
  | node-2   10.6.8.4   10.6.11.4  |
  | command  10.6.8.5   10.6.11.5  |
  | mc-gw    10.6.8.6   10.6.12.1  |
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
csn              1         8             40      CentOS 7.5-1805
node-1           4        16             60      CentOS 7.5-1805
node-2           4        16             60      CentOS 7.5-1805
command          2        16             60      CentOS 7.5-1805
mc-gw            1         8             40      CentOS 7.5-1805
----------------------------------------------------------------
Total           18       128            360
```

