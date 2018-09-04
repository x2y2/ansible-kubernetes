
## 使用说明

1 先确保所有机器SSH互信,hostname设置好，并在所有机器上装有python,rsync

2 把ca.pem,ca-key.pem,ca-config.json,kubernetes-csr.json,admin-csr.json,kube-proxy-csr.json 放到data/ansible-kubernetes/roles/ca/files/csr 中，怎么创建ca.pem ，见[根证书制作](https://github.com/x2y2/kubernetes/blob/master/doc/cert.md)

3 data/ansible-kubernetes/roles/common/vars/main.yaml中，配置需要的变量

```
#/etc/hosts
REGISTRY: 192.168.100.39
ETCD1: 192.168.100.60
ETCD2: 192.168.100.61
ETCD3: 192.168.100.62
MASTER: 192.168.100.60
NODE1: 192.168.100.61
NODE2: 192.168.100.62
#etcd
ETCD_CLUSTER: etcd1=https://192.168.100.60:2380,etcd2=https://192.168.100.61:2380,etcd3=https://192.168.100.62:2380
ETCD_ENDPOINTS: https://192.168.100.60:2379,https://192.168.100.61:2379,https://192.168.100.62:2379
#flanneld-etcd
ETCD_NODE1: 192.168.100.60
#master
MASTER_ADDRESS: 192.168.100.60
HOSTNAME: 192.168.100.60
CLUSTER_IP_RANGE: 10.10.0.1/16
CLUSTER_CIDR: 172.17.0.0/16
API_SERVER: https://192.168.100.60:6443
#kubeconfig
BOOTSTRAP_TOKEN: 1b959fe844b7880d9c97bc2b73d87950
#create clusterrolebinding
MASTER_NODE1: 192.168.100.60
#docker
INSECURE: --insecure-registry=registry:5000
#flannel
ETCD_PREFIX: /kubernetes/network/
INFRA_IMAGES: registry:5000/pause-amd64:3.1
CLUSTER_DNS: 10.10.10.10
```
## 准备可执行二进制文件和证书文件

在roles/ca/files/bin中存放如下文件

```
cfssl  cfssl-certinfo  cfssljson  kubectl
```
roles/ca/files/csr 中存放如下文件

```
admin-csr.json  ca-config.json  ca.csr  ca-csr.json  ca-key.pem  ca.pem  kube-proxy-csr.json  kubernetes-csr.json
```
roles/etcd/files/bin 中存放如下文件

```
etcd  etcdctl
```

roles/master/files/bin中放如下文件

```
kube-apiserver  kube-controller-manager  kube-scheduler
```

roles/nodes/files/bin
```
|-- cni
|   |-- bridge
|   |-- dhcp
|   |-- flannel
|   |-- host-device
|   |-- host-local
|   |-- ipvlan
|   |-- loopback
|   |-- macvlan
|   |-- portmap
|   |-- ptp
|   |-- sample
|   |-- tuning
|   `-- vlan
|-- flanneld
|-- kubelet
|-- kube-proxy
|-- mk-docker-opts.sh
`-- remove-docker0.sh
```

- 本实验在Ubuntu 16.04环境下成功实现