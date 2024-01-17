## Instalação e configuração do Metrics Server

O [**Metrics Server**](https://github.com/kubernetes-sigs/metrics-server) coleta métricas de recursos do [**kubelet**](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/) e as expõem  no [**Kubernetes API server**](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/) por meio da API Metrics.

Baixe a versão mais recente e estável do `metrics-server`.
```bash
curl --location https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.6.2/components.yaml --output metrics-server-v0.6.2.yaml
```

Adicione a flag `--kubelet-insecure-tls` como argumento no container do metrics-server.
```yaml
    spec:
      containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=4443
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        - --metric-resolution=15s
        - --kubelet-insecure-tls
        image: k8s.gcr.io/metrics-server/metrics-server:v0.6.2
```

Implante o metrics-server:
```bash
kubectl create --filename metrics-server-v0.6.2.yaml
```

Aguarde o pod ficar pronto.
```bash
kubectl wait --namespace kube-system --for=condition=ready pod --selector=k8s-app=metrics-server --timeout=90s
```

Após alguns minutos será possível consultar as métrics de recursos do seu cluster. Abaixo dois exemplos simples disso.

```
➜ kubectl top nodes
NAME                            CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%   
k3d-cl-onobrerodrigo-server-0   102m         1%     887Mi           5%
```

```
➜ kubectl top pods --all-namespaces
NAMESPACE        NAME                                        CPU(cores)   MEMORY(bytes)   
default          ingress-nginx-controller-5b5848f778-2lwwl   3m           166Mi           
kube-system      coredns-77ccd57875-mttht                    3m           14Mi            
kube-system      local-path-provisioner-957fdf8bc-mvm2c      1m           8Mi             
kube-system      metrics-server-648b5df564-j29ks             9m           18Mi            
metallb-system   controller-565ccc769f-skpkn                 2m           17Mi            
metallb-system   speaker-tdn96                               3m           17Mi
```
