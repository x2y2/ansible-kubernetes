
## 使用说明
1 先确保所有机器SSH互信,hostname设置好，并在所有机器上装有python,rsync

2 把ca.pem,ca-key.pem,ca-config.json,kubernetes-csr.json,admin-csr.json,kube-proxy-csr.json 放到data/ansible-kubernetes/roles/ca/files/csr 中，怎么创建ca.pem ，见[根证书制作](https://github.com/x2y2/kubernetes/blob/master/doc/cert.md)

3 data/ansible-kubernetes/roles/common/vars/main.yaml中，配置需要的变量

