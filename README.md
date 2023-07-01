

### Kubernetes Cluster Configuration For Kind
```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
  - |-
    kind: ClusterConfiguration
    # configure controller-manager bind address
    controllerManager:
      extraArgs:
        bind-address: 0.0.0.0 #Disable localhost binding
        secure-port: "0"      #Disable the https
        port: "10257"         #Enable http on port 10257
    # configure etcd metrics listen address
    etcd:
      local:
        extraArgs:
          listen-metrics-urls: http://0.0.0.0:2381
    # configure scheduler bind address
    scheduler:
      extraArgs:
        bind-address: 0.0.0.0  #Disable localhost binding
        secure-port: "0"       #Disable the https
        port: "10259"          #Enable http on port 10259
  - |-
    kind: KubeProxyConfiguration
    # configure proxy metrics bind address
    metricsBindAddress: 0.0.0.0
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
- role: worker
- role: worker
```
