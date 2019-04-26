# Contrail POC
# Cluster - OpenShift Origin

# Topology

```
                           br-r1  10.6.11.0/24
                             |
  +---------------------------------+
  | master    10.6.8.1   10.6.11.1  |
  | infra-1   10.6.8.1   10.6.11.1  |
  | node-1    10.6.8.3   10.6.11.3  |
  | node-2    10.6.8.4   10.6.11.4  |
  +---------------------------------+
                  |
                br-int
              10.6.8.254
                  |
               HAProxy

  Contrail web UI:   https://<host>:8143
  OpenShift web UI:  https://<host>:8143
```


# Resource
```
                vCPU    memory(GB)    disk(GB)    OS
master           4        32             80      CentOS 7.5-1805
infra-1          6        64            100      CentOS 7.5-1805
node-1           4        32             40      CentOS 7.5-1805
node-2           4        32             40      CentOS 7.5-1805
----------------------------------------------------------------
Total           18       160            260
```

