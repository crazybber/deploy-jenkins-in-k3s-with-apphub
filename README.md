# deploy-jenkins-in-k3s-with-apphub


创建一个HelmChart CRD

```
apiVersion: helm.cattle.io/v1
kind: HelmChart
metadata:
  name: jenkins
  namespace: kube-system
spec:
  chart: stable/jenkins
  targetNamespace: jenkins
  valuesContent: |-
    Master:
      AdminUser: {{ .adminUser }}
      AdminPassword: {{ .adminPassword }}
    rbac:
      install: true
```

请注意，在元数据中的命名空间用于HelmChart对象。k3s在kube-sysytem中监控CRD对象，如果创建了任一新的HelmChart对象，将启动Helm安装job。



Chart定义要部署哪个repo和Helm Chart。Jenkins应该位于目标命名空间中。我没有使用readme示例中的“set”关键字，而是使用valuesContent，这样可以在其中应用与Chart的value.yaml文件相同的格式。



无需改变Jenkins，将文件另存为jenkins.yaml。创建目标命名空间，并将其作为Kubernetes对象yaml文件应用它。



kubectl create ns jenkins
kubectl apply -f jenkins.yaml


开始监控Helm安装job



kubectl -n kube-system get pods
NAME                            READY   STATUS      RESTARTS   AGE
coredns-7748f7f6df-g6rgw        1/1     Running     0          138m
helm-install-jenkins-txxjn      0/1     Completed   0          111m
helm-install-traefik-bnc5x      0/1     Completed   0          138m
svclb-traefik-b65f58f65-rxllp   2/2     Running     0          138m
traefik-5cc8776646-nfclx        1/1     Running     0          138m


验证PVC是否绑定



kubectl -n jenkins get pvc
NAME      STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
jenkins   Bound    pvc-18988281-4d45-11e9-b75c-5ef9efd9374c   8Gi        RWO            local-path     113m


同时还要验证pod是否正在运行。



kubectl -n jenkins get pods
NAME                             READY   STATUS    RESTARTS   AGE
jenkins-6b6f58bc8d-hbf4r         1/1     Running   0          113m
svclb-jenkins-74fdf6b9f4-zxnwz   1/1     Running   0          113m
