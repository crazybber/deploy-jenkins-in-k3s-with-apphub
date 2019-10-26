# install k3s

method 1:

```bash
curl -sfL https://get.k3s.io | sh -
```

you will see:

```bash
[INFO]  Finding latest release
[INFO]  Using v0.10.0 as release
[INFO]  Downloading hash https://github.com/rancher/k3s/releases/download/v0.10.0/sha256sum-amd64.txt
[INFO]  Downloading binary https://github.com/rancher/k3s/releases/download/v0.10.0/k3s
[INFO]  Verifying binary download
[INFO]  Installing k3s to /usr/local/bin/k3s
which: no kubectl in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin)
[INFO]  Creating /usr/local/bin/kubectl symlink to k3s
which: no crictl in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin)
[INFO]  Creating /usr/local/bin/crictl symlink to k3s
which: no ctr in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin)
[INFO]  Creating /usr/local/bin/ctr symlink to k3s
[INFO]  Creating killall script /usr/local/bin/k3s-killall.sh
[INFO]  Creating uninstall script /usr/local/bin/k3s-uninstall.sh
[INFO]  env: Creating environment file /etc/systemd/system/k3s.service.env
[INFO]  systemd: Creating service file /etc/systemd/system/k3s.service
[INFO]  systemd: Enabling k3s unit
Created symlink from /etc/systemd/system/multi-user.target.wants/k3s.service to /etc/systemd/system/k3s.service.
[INFO]  systemd: Starting k3s
```

method 2:

```bash
curl -sfL https://github.com/rancher/k3s/releases/download/vX.Y.Z/k3s -o /usr/local/bin/k3s
chmod 0755 /usr/local/bin/k3s

curl -sfL https://get.k3s.io -o install-k3s.sh
chmod 0755 install-k3s.sh

export INSTALL_K3S_SKIP_DOWNLOAD=true
./install-k3s.sh
···

```bash
kubectl get nodes
NAME               STATUS   ROLES    AGE     VERSION
hostname   Ready    master   3m32s   v1.16.2-k3s.1
```

K3S_TOKEN is created at /var/lib/rancher/k3s/server/node-token

## On a different node run the below. NODE_TOKEN comes from /var/lib/rancher/k3s/server/node-token on your server

```bash
k3s agent --server https://myserver:6443 --token ${NODE_TOKEN}
```

## Accessing Cluster from Outside

Copy /etc/rancher/k3s/k3s.yaml on your machine located outside the cluster as ~/.kube/config. Then replace “localhost” with the IP or name of your k3s server. kubectl can now manage your k3s cluster.
