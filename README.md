[![Build Status](https://travis-ci.org/kubernetes-csi/drivers.svg?branch=master)](https://travis-ci.org/kubernetes-csi/drivers)
# CSI NFS Driver

These drivers are provided purely for illustrative purposes, and should not be used for production workloads.

## Other sample drivers
Please read [Drivers](https://kubernetes-csi.github.io/docs/Drivers.html) for more information

## Kubernetes
### Requirements

The folllowing feature gates and runtime config have to be enabled to deploy the driver

```
FEATURE_GATES=CSIPersistentVolume=true,MountPropagation=true
RUNTIME_CONFIG="storage.k8s.io/v1alpha1=true"
```

Mountprogpation requries support for privileged containers. So, make sure privileged containers are enabled in the cluster.

### Example local-up-cluster.sh

```ALLOW_PRIVILEGED=true FEATURE_GATES=CSIPersistentVolume=true,MountPropagation=true RUNTIME_CONFIG="storage.k8s.io/v1alpha1=true" LOG_LEVEL=5 hack/local-up-cluster.sh```

### Deploy

```kubectl -f deploy/kubernetes create```

### Example Nginx application
Please update the NFS Server & share information in nginx.yaml file.

```kubectl -f examples/kubernetes/nginx.yaml create```

## Using CSC tool

### Build nfsplugin
```
$ make nfs
```

### Start NFS driver
```
$ sudo ./_output/nfsplugin --endpoint tcp://127.0.0.1:10000 --nodeid CSINode -v=5
```

## Test
Get ```csc``` tool from https://github.com/rexray/gocsi/tree/master/csc

#### Get plugin info
```
$ csc identity plugin-info --endpoint tcp://127.0.0.1:10000
"NFS"	"0.1.0"
```

#### NodePublish a volume
```
$ export NFS_SERVER="Your Server IP (Ex: 10.10.10.10)"
$ export NFS_SHARE="Your NFS share"
$ csc node publish --endpoint tcp://127.0.0.1:10000 --target-path /mnt/nfs --attrib server=$NFS_SERVER --attrib share=$NFS_SHARE nfstestvol
nfstestvol
```

#### NodeUnpublish a volume
```
$ csc node unpublish --endpoint tcp://127.0.0.1:10000 --target-path /mnt/nfs nfstestvol
nfstestvol
```

#### Get NodeID
```
$ csc node get-id --endpoint tcp://127.0.0.1:10000
CSINode
```

## Community, discussion, contribution, and support

Learn how to engage with the Kubernetes community on the [community page](http://kubernetes.io/community/).

You can reach the maintainers of this project at:

- [Slack channel](https://kubernetes.slack.com/messages/sig-storage)
- [Mailing list](https://groups.google.com/forum/#!forum/kubernetes-sig-storage)

### Code of conduct

Participation in the Kubernetes community is governed by the [Kubernetes Code of Conduct](code-of-conduct.md).

[owners]: https://git.k8s.io/community/contributors/guide/owners.md
[Creative Commons 4.0]: https://git.k8s.io/website/LICENSE
