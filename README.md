
# Vagrantfile and Scripts to Automate Kubernetes Setup on containerd runtime

## Prerequisites
1. You need to have vagrant installed https://developer.hashicorp.com/vagrant/downloads
2.  A vitualbox setup that can support 3 nodes
        - 1 master: 4G RAM, 2 vCPUS
        - 2 worker: Each 2G RAM, 1 vCPUS

## Bring Up the Cluster

To provision the cluster, execute the following commands.

``` shell
git clone https://github.com/kaygee500/k8s-containerd-vagrant.git
cd k8s-containerd-vagrant
vagrant up
```

## Accessing the cluster from your local machine

```shell 
Install kublet on your machine https://kubernetes.io/docs/tasks/tools/
```
```shell
cd k8s-containerd-vagrant
cd configs
export KUBECONFIG=$(pwd)/config
```

or you can copy the config file to .kube directory.

```shell
cp config ~/.kube/
```

## Install Kubernetes Dashboard

The dashboard is automatically installed by default, but it can be skipped by commenting out Lines 45 to 48 in the vagrant file.You can then deploy it later by enabling it in:
```shell
vagrant ssh -c "/vagrant/scripts/dashboard.sh" master
```

## Kubernetes Dashboard Access

To get the login token, copy it from _config/token_ or run the following command:
```shell
kubectl -n kubernetes-dashboard get secret/admin-user -o go-template="{{.data.token | base64decode}}"
```

Proxy the dashboard:
```shell
kubectl proxy
```

Open the site in your browser:
```shell
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/overview?namespace=kubernetes-dashboard
```

## Shutting the cluster,
``` shell
vagrant halt
```

## Restarting the cluster,
``` shell
vagrant up
```

## Detroying the cluster,
``` shell
vagrant destroy -f
```

