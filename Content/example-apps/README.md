# Aplicação de Exemplo

Aqui disponibilizo de forma rápida uma aplicação de fácil instalação e que proporciona um cenário bom para estudos sobre Kubernetes.

**Wordpress**

O [`Wordpress`](https://wordpress.org/) é sem dúvidas o CMS mais utilizado na internet.

Para executar o Wordpress corretamente, primeiro execute o `yaml` do `MariaDB`.
```bash
kubectl create -f mariadb.yaml
```
Aguarde até que o pod do `MariaDB` esteja disponível.

Validado que o pod esteja executando, vamos agora executar o `yaml` do `Wordpress`.

```bash
kubectl create -f wordpress.yaml
```
Aguarde até que os pod do `Wordpress` esteja disponpivel.

> Confira o arquivo `mariadb.yaml` e `wordpress.yaml` para entender ou modificar as configurações que desejar.

No arquivo `wordpress.yaml` no objeto do ingress, o parâmetro `host` recebe o valor `meudominio.local`. Sendo assim, você precisa adicionar no `/etc/host` do seu computador a entrada abaixo:

```
172.27.0.30 meudominio.local
```
> O IP acima, `172.27.0.30` é o IP atribuído ao LoadBalancer do Nginx Ingress Controller.

Para consultar qual IP atribuído ao LoadBalancer do Nginx Ingress Controller execute o comando abaixo:

```bash
kubectl get service --namespace ingress-nginx ingress-nginx-controller
```
Se o Ingress estiver sido instalado atráves do `yaml`

ou
```bash
kubectl get service --namespace default ingress-nginx-controller
```
Se o Ingress foi instalado a partir do `Helm`.

O resultado será semelhante a este mostrado abaixo:

```
NAME                       TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
ingress-nginx-controller   LoadBalancer   10.96.122.185   172.27.0.30   80:31598/TCP,443:30455/TCP   170m
```

Pronto, o IP mostrado no `EXTERNAL-IP` será o IP que você utilizará como entrada de acesso para as aplicações.
