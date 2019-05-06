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
* [IP Fabric Remote](underlay/fabric-remote.md)
* [Gateway x2](underlay/gateway-x2.md)
  * Contrail gateway to corp gateway.
  * Contrail gateway to Contrail gateway (DCI).
  * Remote compute.


### Cluster
* [Contrail and Openstack](cluster/openstack/openstack.md)
* [Contrail and Openstack HA](cluster/openstack-ha/openstack-ha.md)
* [Contrail and Kubernetes](cluster/kubernetes/kubernetes.md)
* [Contrail and Kubernetes HA](cluster/kubernetes-ha/kubernetes-ha.md)
* [Contrail and OpenShift Origin](cluster/openshift-origin/openshift-origin.md)
* [Contrail Fabric Management](cluster/cfm/cfm.md)
* [Contrail Fabric Management](cluster/cfm/cfm.md)
* [Contrail Fabric Management Remote](cluster/cfm-remote/cfm-remote.md)


# 2 Hypervisor

Here are prerequisites for the hypervisor.

* Resource
  * The hypervisor used by this guide has 32 vCPU, 256GB memory and 2TB disk.
  * At least one NIC that has public/internet access.

* Host OS
  * CentOS 7.5 and Ubuntu 16.04.3 are validated.

* Disk partition
  * This has to be done during host OS installation.
  * Use logical volume.
  * Make single root `/` LV.
  * Don't create separate home `/home` LV.
  * Optionally, for better disk performance, reserve a partition (1TB) for libvirt volume.

* Kernel
  * Upgrade kernel and reboot, after installation.


# 3 Setup hypervisor

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

* Setup the hypervisor.
```
cd contrail-poc
poc setup-hypervisor
```

* Copy `vm-image` to `/opt` directory.
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


# 5 Notes

## 5.1 vQFX

This POC is using these vQFX RE and PFE image.
```
jinstall-vqfx-10-f-18.1R1.9.img
cosim_20180212.qcow2
```

With `jinstall-vqfx-10-f-18.4R1.8.qcow2`, EVPN type-2 MAC/IP route with IP shows up in `default-switch.evpn.0`. ARP request from BMS is resolved by vQFX, not being multicasted anymore. When VM sends out ARP request for BMS, vrouter doesn't resolve it, but multicasts ARP request. When vQFX resolves request and sends reply back to vrouter, it sets VNI to 0. This is invalid VNI for vrouter. So the reply is dropped by vrouter.



