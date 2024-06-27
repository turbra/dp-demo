# Creating the OpenStack Control Plane

We will be using a preconfigured file(files/osp-ng-ctlplane-deploy.yaml) to
create the control plane and at the same time enable the required services,
configure the control plane network, and configure the service back ends. The
bastion has a preconfigured NFS service that we will be using to store glance
images and cinder volumes.

# Create a nfs share for cinder
```
mkdir /nfs/cinder
chmod 777 /nfs/cinder
```

# Configure nfs storage class

```
mkdir /nfs/pv6
mkdir /nfs/pv7
mkdir /nfs/pv8
mkdir /nfs/pv9
mkdir /nfs/pv10
mkdir /nfs/pv11
chmod 777 /nfs/pv*
```

Update `server:` with your relevant IP

```
oc create -f nfs-storage.yaml
```

# Prepare the secret to place the NFS server connection used by Cinder
```
oc create secret generic cinder-nfs-config --from-file=nfs-cinder-conf
```
# Prepare the secret to place the server connection used by Glance
```
oc create secret generic glance-cinder-config --from-file=glance-conf
```

# Finally create the OpenStack control plane
```
oc create -f osp-ng-ctlplane-deploy.yaml
```

Note this expects a storage class named `ocs-storagecluster-ceph-rbd`
```
LAST SEEN   TYPE      REASON               OBJECT                                                           MESSAGE
5s          Warning   ProvisioningFailed   persistentvolumeclaim/mysql-db-openstack-cell1-galera-0          storageclass.storage.k8s.io "ocs-storagecluster-ceph-rbd" not found
5s          Warning   ProvisioningFailed   persistentvolumeclaim/mysql-db-openstack-galera-0                storageclass.storage.k8s.io "ocs-storagecluster-ceph-rbd" not found
5s          Warning   ProvisioningFailed   persistentvolumeclaim/ovndbcluster-nb-etc-ovn-ovsdbserver-nb-0   storageclass.storage.k8s.io "ocs-storagecluster-ceph-rbd" not found
5s          Warning   ProvisioningFailed   persistentvolumeclaim/ovndbcluster-sb-etc-ovn-ovsdbserver-sb-0   storageclass.storage.k8s.io "ocs-storagecluster-ceph-rbd" not found
5s          Warning   ProvisioningFailed   persistentvolumeclaim/persistence-rabbitmq-cell1-server-0        storageclass.storage.k8s.io "ocs-storagecluster-ceph-rbd" not found
5s          Warning   ProvisioningFailed   persistentvolumeclaim/persistence-rabbitmq-server-0              storageclass.storage.k8s.io "ocs-storagecluster-ceph-rbd" not found
```
[back](network-isolation.md) [next](create-dp.md)
