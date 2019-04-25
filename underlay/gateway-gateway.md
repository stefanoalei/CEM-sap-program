# Contrail Virtual Fabric
# Underlay - Gateway to Gateway

# Topology

```
   +-------------+
   |  ext-host   |
   | 172.16.1.10 |
   +-------------+
          |         provider-network-1: 172.16.11.0/24
          |         provider-network-2: 172.16.12.0/24
  +-----------------+-----------------+
  |  xe-0/0/1       |                 |
  |  172.16.1.254   |                 |
  |                                   |
  | vmx-2 (External Gateway)          |
  |  em0: 10.6.8.41                   |
  |  lo0: 10.6.0.41                   |
  |  ASN: 64041                       |
  |                                   |
  |  10.6.30.1/31   |                 |
  |  xe-0/0/0       |                 |
  +-----------------+-----------------+
        |
 =======+==================================
        |
  +-----------------+-----------------+
  |  xe-0/0/0       |                 |
  |  10.6.30.0/31   |                 |
  |                                   |
  | vmx-1 (Contrail Gateway)          |
  |  em0: 10.6.8.31                   |
  |  lo0: 10.6.0.31                   |
  |  ASN: 64031                       |
  |                                   |
  |  10.6.20.0/31   |                 |
  |  xe-0/0/1       |                 |
  +-----------------+-----------------+
        |
        |
  +----------------+------------------+
  |  xe-0/0/0      |                  |
  |  10.6.20.1/31  |                  |
  |                                   |
  | vqfx-leaf-1                       |
  |  em0: 10.6.8.11                   |
  |  lo0: 10.6.0.11                   |
  |  ASN: 64011                       |
  |                                   |
  |  xe-0/0/1  |  xe-0/0/2 | xe-0/0/3 |
  +------------+-----------+----------+
       |
     br-r1
       |
  +---------+
  | cluster |
  +---------+
       |
       |                    management:      10.6.8.0/24
     br-int                 loopback:        10.6.0.0/24
   10.6.8.254               gateway-leaf:    10.6.20.0/24
       |                    gateway-gateway: 10.6.30.0/24
       |                    rack-1:          10.6.11.0/24
    HAProxy

  Contrail web UI:   https://<host>:8143
  Contrail Command:  https://<host>:9091
  OpenStack Horizon: http://<host>
```


# Resource
```
                  vCPU    memory(GB)    disk(GB)    OS
contrail           4        64            150      CentOS 7.5-1805
openstack          4        64            150      CentOS 7.5-1805
csn                2        16             80      CentOS 7.5-1805
compute-1          4        16             80      CentOS 7.5-1805
compute-2          4        16             80      CentOS 7.5-1805
ext-host           1         2             20      CentOS 7.5-1805
vqfx-leaf-1-re     1         1                     Junos 18.4R1.8
vqfx-leaf-1-pfe    1         2                     Junos 18.4R1.8
vmx-1-re           1         1                     Junos 18.3R1.9
vmx-1-pfe          4         2                     Junos 18.3R1.9
vmx-2-re           1         1                     Junos 18.3R1.9
vmx-2-pfe          4         2                     Junos 18.3R1.9
----------------------------------------------------------------
Total              31       187
```

