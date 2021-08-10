# k8s-certs-management

# certs renewal
This explains how to renew k8s certs using kubeadm.

## 1. check current expiry date for certs

`kubeadm alpha certs check-expiration

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

`

## 2. backup cluster certs directories and conf files

`
cp -R /etc/kubernetes/ssl /etc/kubernetes/ssl.backup <br />
cp -R  /etc/ssl/etcd/ /etc/ssl/etcd.backup <br />
cp -R  /etc/kubernetes/pki/ /etc/kubernetes/pki.backup <br />
cp /etc/kubernetes/admin.conf /etc/kubernetes/admin.conf.backup <br />
cp /etc/kubernetes/controller-manager.conf /etc/kubernetes/controller-manager.conf.backup <br />
cp /etc/kubernetes/kubelet.conf /etc/kubernetes/kubelet.conf.backup <br /> 
cp /etc/kubernetes/scheduler.conf /etc/kubernetes/scheduler.conf.backup <br />
`

## 3. renew certificates

`kubeadm alpha certs renew all`

