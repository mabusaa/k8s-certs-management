# k8s certs renewal
This explains how to renew k8s certs using kubeadm.

## kubeadm certs commands
k8s version 1.20+ ``` kubeadm certs```

k8s version < 1.20 ``` kubeadm alpha certs```

## 1. check current expiry date for certs

```kubeadm alpha certs check-expiration```

```

CERTIFICATE                EXPIRES                  RESIDUAL TIME   CERTIFICATE AUTHORITY   EXTERNALLY MANAGED
admin.conf                 Aug 27, 2021 12:16 UTC   16d                                     no
apiserver                  Aug 27, 2021 12:14 UTC   16d             ca                      no
apiserver-kubelet-client   Aug 27, 2021 12:14 UTC   16d             ca                      no
controller-manager.conf    Aug 27, 2021 12:16 UTC   16d                                     no
front-proxy-client         Aug 27, 2021 12:14 UTC   16d             front-proxy-ca          no
scheduler.conf             Aug 27, 2021 12:16 UTC   16d                                     no

CERTIFICATE AUTHORITY   EXPIRES                  RESIDUAL TIME   EXTERNALLY MANAGED
ca                      Aug 25, 2030 12:14 UTC   9y              no
front-proxy-ca          Aug 25, 2030 12:14 UTC   9y              no

```

## 2. backup cluster certs directories and conf files

```
cp -R /etc/kubernetes/ssl /etc/kubernetes/ssl.backup \
cp -R  /etc/ssl/etcd/ /etc/ssl/etcd.backup \
cp -R  /etc/kubernetes/pki/ /etc/kubernetes/pki.backup \
cp /etc/kubernetes/admin.conf /etc/kubernetes/admin.conf.backup \
cp /etc/kubernetes/controller-manager.conf /etc/kubernetes/controller-manager.conf.backup \
cp /etc/kubernetes/kubelet.conf /etc/kubernetes/kubelet.conf.backup \
cp /etc/kubernetes/scheduler.conf /etc/kubernetes/scheduler.conf.backup 
```

## 3. renew certificates

```kubeadm alpha certs renew all```

## 4. verify the new expiry date for certs

```kubeadm alpha certs check-expiration```
```
CERTIFICATE                EXPIRES                  RESIDUAL TIME   CERTIFICATE AUTHORITY   EXTERNALLY MANAGED
admin.conf                 Aug 10, 2022 18:01 UTC   364d                                    no
apiserver                  Aug 10, 2022 18:01 UTC   364d            ca                      no
apiserver-kubelet-client   Aug 10, 2022 18:01 UTC   364d            ca                      no
controller-manager.conf    Aug 10, 2022 18:01 UTC   364d                                    no
front-proxy-client         Aug 10, 2022 18:01 UTC   364d            front-proxy-ca          no
scheduler.conf             Aug 10, 2022 18:01 UTC   364d                                    no

CERTIFICATE AUTHORITY   EXPIRES                  RESIDUAL TIME   EXTERNALLY MANAGED
ca                      Aug 25, 2030 12:14 UTC   9y              no
front-proxy-ca          Aug 25, 2030 12:14 UTC   9y              no
```
## 5. get the nodes

verify that you can get the nodes

```
kubectl get no

NAME                                 STATUS                     ROLES    AGE    VERSION
node1                                Ready                      master   343d   v1.18.8
node2                                Ready                      master   343d   v1.18.8
node3                                Ready                      master   348d   v1.18.8
node4                                Ready                      worker   348d   v1.18.8
node5                                Ready                      worker   348d   v1.18.8
node6                                Ready                      worker   348d   v1.18.8


```

