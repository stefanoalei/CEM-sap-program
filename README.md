# Contrail POC

# 1 Overview

Pick cluster and underlay to build the POC.

### Underlay
* [Bridge](underlay/bridge.md)
  * Contrail integration deployment.
  * Contrail basic features.
* [Gateway MX](underlay/gateway-mx.md)
  * Overlay-underlay connectivity via MX.
  * Distributed fabric forwarding (gateway-less mode).
* [IP Fabric](underlay/fabric.md)
  * Contrail Fabric Management
* [IP Fabric HA](underlay/fabric-ha.md)
  * Contrail Fabric Management HA
* [Multi-Site](underlay/multi-site.md)
* [Gateway x2](underlay/gateway-x2.md)
  * Contrail gateway to corp gateway.
  * Contrail gateway to Contrail gateway (DCI).
  * Remote compute.


### Cluster
* [Contrail and Openstack](#a1-openstack)
* [Contrail and Openstack HA](#a2-openstack-ha)
* [Contrail Fabric Management](#a3-cfm)
* [Contrail and Kubernetes](#a4-kubernetes)
* [Contrail and Kubernetes HA](#a5-kubernetes-ha)
* [Contrail and OpenShift Origin](#a6-openshift)
* [Contrail and OpenShift Origin HA](#a7-openshift-ha)
* [Multi-site 1](#a8-multi-site)
* [Multi-site 2](#a9-multi-site-2)
* [Multi-cloud Kubernetes](#a10-multi-cloud-kubernetes)


# 2 Hypervisor

Here are prerequisites for the hypervisor.

* Resource
  * The hypervisor used by this guide has 32 vCPU, 256GB memory and 2TB disk.
  * At least one NIC that has public/internet access.

* Host OS
  * CentOS 7.5 and Ubuntu 16.04.3 are validated.

* Disk partition
  * This has to be done during host OS installation.
  * Optionally, for better disk performance, reserve a partition (1TB) for libvirt volume.
  * Here is the recommended partition schema for a 2TB disk.
    * Partition 1, 250MB, `/boot`.
    * Partition 2, 1TB, volume group `system`.
      * Logical volume `swap`, 256GB.
      * Logical volume `root`, the rest space.
    * Partition 3, the rest space (for libvirt volume).

* Kernel
  * Upgrade kernel and reboot, after installation.


# 3 Build enviroment

## 3.1 Setup hypervisor

* Install `git`.
```
yum install git
```

* Clone repository.
```
git clone https://github.com/tonyliu0592/contrail-poc.git
```

* Update the followings variables in `poc.conf`.
  * `volume_dev`
  * `ntp_server`
Note, comment out `volume_dev` if no partition is reserved.

* Setup the hypervisor.
```
cd contrail-poc
poc setup-hypervisor
```


## 3.2 VM image

Copy the following VM images `/opt/vm-image` directory.
```
CentOS-7-x86_64-GenericCloud-1805.qcow2
cirros-0.4.0-x86_64-disk.img
vmx-re.qcow2
vmx-re-hdd.qcow2
metadata-usb-re.img
vFPC-20180829.img
vqfx-pfe.qcow2
vqfx-re.qcow2
```


## 3.3 AppFormix

AppFormix is integrated with cluster `cfm`.

* Download the following packages from Juniper and upload them to `/opt/appformix` directory on the hypervisor host.
```
appformix-3.0.0.tar.gz
appformix-dependencies-images-3.0.0.tar.gz
appformix-openstack-images-3.0.0.tar.gz
appformix-platform-images-3.0.0.tar.gz
```

* Send request to `AppFormix-Key-Request.juniper.net` for AppFormix license.

* Copy the license to `/opt/appformix` directory on the hypervisor host.

* Update `appformix_license` in `poc.conf`.

#### Note
For the first time to open AppFormix UI, select `skip license upload` and `skip the installation`.


# 4 Deploy

## 4.1 Build underlay and cluster

* Update variables in `poc.conf` for deployment.
  * Choose underlay and cluster.
  * Set username and password for hub.juniper.net/contrail.
  * Set Contrail container tag and Contrail Command container tag.
  * Set Ansible playbook name.

* Build underlay and cluster.
```
poc build-underlay
poc build-cluster
```


## 4.2 Change underlay

* Delete existing underlay.
```
poc delete-underlay
```
#### Note: Delete underlay before updating `poc.conf`.

* Update underlay selection in `poc.conf`.

* Build new underlay.
```
poc build-underlay
```


## 4.3 Change cluster

* Delete existing cluster.
```
poc delete-cluster
```
#### Note: Delete cluster before updating `poc.conf`.

* Update cluster selection in `poc.conf`.

* Build new cluster.
```
poc build-cluster
```


# 5 Access

To access server or underlay device, login onto the host, then ssh to the hostname or management address.

HAProxy is configured to provide access to the cluster.
```
Contrail web UI:   https://<host>:8143
Contrail Command:  https://<host>:9091
OpenStack Horizon: http://<host>
AppFormix UI:      http://<host>:9000
```

Default username and password for vQFX and vMX is `root` / `Juniper`.


# 6 Notes

## 6.1 vQFX

This POC is using these vQFX RE and PFE image.
```
jinstall-vqfx-10-f-18.1R1.9.img
cosim_20180212.qcow2
```

With `jinstall-vqfx-10-f-18.4R1.8.qcow2`, EVPN type-2 MAC/IP route with IP shows up in `default-switch.evpn.0`. ARP request from BMS is resolved by vQFX, not being multicasted anymore. When VM sends out ARP request for BMS, vrouter doesn't resolve it, but multicasts ARP request. When vQFX resolves request and sends reply back to vrouter, it sets VNI to 0. This is invalid VNI for vrouter. So the reply is dropped by vrouter.


# Appendix A Cluster

## A.1 openstack
```
host          management   data       vCPU  memory   disk  OS
---------------------------------------------------------------------------
contrail-1    10.6.8.1     10.6.11.1     5      64    100  CentOS 7.5-1805
openstack-1   10.6.8.2     10.6.11.2     5      48    100  CentOS 7.5-1805
compute-1     10.6.8.7     10.6.11.7     4      16     60  CentOS 7.5-1805
compute-2     10.6.8.8     10.6.11.8     4      16     60  CentOS 7.5-1805
---------------------------------------------------------------------------
Total                                   18     144    320
```


## A.2 openstack-ha
```
host          management   data       vCPU  memory   disk  OS
---------------------------------------------------------------------------
contrail-1    10.6.8.1     10.6.11.1     5      64    100  CentOS 7.5-1805
contrail-2    10.6.8.3     10.6.11.3     5      64    100  CentOS 7.5-1805
contrail-3    10.6.8.5     10.6.11.5     5      64    100  CentOS 7.5-1805
openstack-1   10.6.8.2     10.6.11.2     5      48    100  CentOS 7.5-1805
openstack-2   10.6.8.4     10.6.11.4     5      48    100  CentOS 7.5-1805
openstack-3   10.6.8.6     10.6.11.6     5      48    100  CentOS 7.5-1805
compute-1     10.6.8.7     10.6.11.7     4      16     60  CentOS 7.5-1805
compute-2     10.6.8.8     10.6.11.8     4      16     60  CentOS 7.5-1805
---------------------------------------------------------------------------
Total                                   38     368    720
```


## A.3 cfm
```
host          management   data       vCPU  memory   disk  OS
---------------------------------------------------------------------------
contrail-1    10.6.8.1     10.6.11.1     5      64    100  CentOS 7.5-1805
openstack-1   10.6.8.2     10.6.11.2     5      48    100  CentOS 7.5-1805
csn-1         10.6.8.3     10.6.11.3     1       8     40  CentOS 7.5-1805
compute-1     10.6.8.7     10.6.11.7     4      16     60  CentOS 7.5-1805
---------------------------------------------------------------------------
Total                                   15     136    300
```


## A.4 kubernetes
```
host          management   data       vCPU  memory   disk  OS
---------------------------------------------------------------------------
kmaster-1     10.6.8.1     10.6.11.1     5      64    100  CentOS 7.5-1805
node-1        10.6.8.7     10.6.11.7     4      16     60  CentOS 7.5-1805
node-2        10.6.8.8     10.6.11.8     4      16     60  CentOS 7.5-1805
---------------------------------------------------------------------------
Total                                   13      96    220
```


## A.5 kubernetes-ha
```
host          management   data       vCPU  memory   disk  OS
---------------------------------------------------------------------------
kmaster-1     10.6.8.1     10.6.11.1     5      64    100  CentOS 7.5-1805
kmaster-1     10.6.8.2     10.6.11.2     5      64    100  CentOS 7.5-1805
kmaster-1     10.6.8.3     10.6.11.3     5      64    100  CentOS 7.5-1805
node-1        10.6.8.7     10.6.11.7     4      16     60  CentOS 7.5-1805
node-2        10.6.8.8     10.6.11.8     4      16     60  CentOS 7.5-1805
---------------------------------------------------------------------------
Total                                   23     224    420
```


## A.6 openshift
```
host          management   data       vCPU  memory   disk  OS
---------------------------------------------------------------------------
master-1      10.6.8.1     10.6.11.1     2      16     60  CentOS 7.5-1805
infra-1       10.6.8.4     10.6.11.4     5      64    100  CentOS 7.5-1805
node-1        10.6.8.7     10.6.11.7     4      16     60  CentOS 7.5-1805
node-2        10.6.8.8     10.6.11.8     4      16     60  CentOS 7.5-1805
---------------------------------------------------------------------------
Total                                   15     112    280
```


## A.7 openshift-ha
```
host          management   data       vCPU  memory   disk  OS
---------------------------------------------------------------------------
master-1      10.6.8.1     10.6.11.1     2      16     60  CentOS 7.5-1805
master-2      10.6.8.2     10.6.11.2     2      16     60  CentOS 7.5-1805
master-3      10.6.8.3     10.6.11.3     2      16     60  CentOS 7.5-1805
infra-1       10.6.8.4     10.6.11.4     5      64    100  CentOS 7.5-1805
infra-2       10.6.8.5     10.6.11.5     5      64    100  CentOS 7.5-1805
infra-3       10.6.8.6     10.6.11.6     5      64    100  CentOS 7.5-1805
node-1        10.6.8.7     10.6.11.7     4      16     60  CentOS 7.5-1805
node-2        10.6.8.8     10.6.11.8     4      16     60  CentOS 7.5-1805
---------------------------------------------------------------------------
Total                                   29     272    600
```


## A.8 multi-site
```
host          management   data       vCPU  memory   disk  OS
---------------------------------------------------------------------------
contrail-1    10.6.8.1     10.6.11.1     5      64    100  CentOS 7.5-1805
openstack-1   10.6.8.2     10.6.11.2     5      48    100  CentOS 7.5-1805
csn-1         10.6.8.3     10.6.11.3     1       8     40  CentOS 7.5-1805
compute-1     10.6.8.7     10.6.11.7     4      16     60  CentOS 7.5-1805
compute-r1    10.6.8.8     10.6.12.8     4      16     60  CentOS 7.5-1805
---------------------------------------------------------------------------
Total                                   19     152    360
```


## A.9 multi-site-2
```
host            management   data       vCPU  memory   disk  OS
---------------------------------------------------------------------------
contrail-c2-1   10.6.8.51    10.6.13.1     5      64    100  CentOS 7.5-1805
openstack-c2-1  10.6.8.52    10.6.13.2     5      48    100  CentOS 7.5-1805
csn-c2-1        10.6.8.53    10.6.13.3     1       8     40  CentOS 7.5-1805
compute-c2-1    10.6.8.57    10.6.13.7     4      16     60  CentOS 7.5-1805
compute-c2-r1   10.6.8.58    10.6.14.8     4      16     60  CentOS 7.5-1805
---------------------------------------------------------------------------
Total                                     19     152    360
```

# Appendix B Management address

## B.1 Underlay management address
```
            fabric-ha     fabric         multi-site     multi-site-2
-------------------------------------------------------------------------
10.6.8.11  vqfx-leaf-1    vqfx-leaf-1    vqfx-leaf-1
10.6.8.12  vqfx-leaf-2    vqfx-leaf-2    vqfx-leaf-2
10.6.8.13                                               vqfx-leaf-3
10.6.8.14                                               vqfx-leaf-4
10.6.8.21  vqfx-spine-1   vqfx-spine-1   vqfx-spine-1
10.6.8.22  vqfx-spine-2                                 vqfx-spine-2
10.6.8.31  vmx-1          vmx-1          vmx-1
10.6.8.32  vmx-2                         vmx-2
10.6.8.33                                               vmx-3
10.6.8.34                                               vmx-4
```

## B.2 Cluster management address
```
            openstack-ha  openstack     cfm           kubernetes-ha  kubernetes  openshift  multi-cloud-kubernetes  multi-site   multi-site-2
------------------------------------------------------------------------------------------------------------------------
10.6.8.1    contrail-1    contrail-1    contrail-1    kmaster-1      kmaster-1   master-1   kmaster-1               contrail-1
10.6.8.2    openstack-1   openstack-1   openstack-1   kmaster-2                                                     openstack-1
10.6.8.3    contrail-2                  csn-1         kmaster-3                                                     csn-1
10.6.8.4    openstack-2                                                          infra-1                            csn-r1
10.6.8.5    contrail-3
10.6.8.6    openstack-3
10.6.8.7    compute-1     compute-1     compute-1     node-1         node-1      node-1     node-1                  compute-1
10.6.8.8    compute-2     compute-2                   node-2         node-2      node-2     node-2                  compute-r1
10.6.8.9                                                                                    mc-gw
10.6.8.10                                                                                   command

10.6.8.51                                                                                                                        contrail-c2-1
10.6.8.52                                                                                                                        openstack-c2-1
10.6.8.53                                                                                                                        csn-c2-1
10.6.8.54                                                                                                                        csn-c2-r1
10.6.8.57                                                                                                                        compute-c2-1
10.6.8.58                                                                                                                        compute-c2-r1
```


