## Instalação e configuração do Nginx Ingress Controller

Aqui faremos a implantação do [**Ingress Controller**](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/) chamado [**Nginx Ingress Controller**](https://kubernetes.github.io/ingress-nginx/) de duas formas:

- Utilizando o próprio manifesto de instalação;
- Utilizando o gerenciador de pacotes `helm`.


### Instalação do Ingress Controller atráves do manifesto.

Dessa forma podemos trabalhar com recursos do tipo [**Ingress**](https://kubernetes.io/docs/concepts/services-networking/ingress/).

A implantação padrão do **Nginx Ingress Controller** é bem simples, bastando executar o comando abaixo:
```bash
kubectl apply --filename https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.3/deploy/static/provider/cloud/deploy.yaml
```

Verifique se o serviço `ingress-nginx-controller` pegou o IP do pool `development` configurado no `MetalLB`.
```bash
kubectl get service --namespace ingress-nginx ingress-nginx-controller
```

O resultado será semelhante a este abaixo:
```
NAME                       TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)                      AGE
ingress-nginx-controller   LoadBalancer   10.96.37.60   172.19.0.30   80:31193/TCP,443:30981/TCP   59s
```

No browser, informe o IP mostrado no `EXTERNAL-IP` e você será redirecionado para o `default backend` que apenas mostra uma tela com 404. Está funcional.

### Instalação do Ingress Controller atráves do Helm.

Não irei abordar a instalação do Helm, mas a documentação é muito explícita de como fazer.

[Instalando o Helm](https://helm.sh/docs/intro/install/)

Sabendo que o Helm foi instalado com sucesso, basta executar estes comandos para que o Ingress Controller seja instalado em seu cluster.
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```
```bash
helm repo update
```
E por final:
```bash
 helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress-nginx --create-namespace --version 4.10.3
 ```

 Essa versão do Nginx Ingress Controller é compativel com as versões 1.30, 1.29, 1.28, 1.27 e 1.26 do Kubernetes.

 E finalmente, verifique se o serviço `ingress-nginx-controller` pegou o IP do pool `development` configurado no `MetalLB`.
```bash
kubectl get service --namespace default ingress-nginx-controller
```

O resultado será semelhante a este abaixo:
```
NAME                       TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)                      AGE
ingress-nginx-controller   LoadBalancer   10.96.37.60   172.19.0.30   80:31193/TCP,443:30981/TCP   59s
```