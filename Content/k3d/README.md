## Instalação e Configuração do K3D.

<p align="center">
  <img width="500" height="350" src="https://d33wubrfki0l68.cloudfront.net/d0c94836ab5b896f29728f3c4798054539303799/9f948/logo/logo.png">
</p>

O [**K3D**](https://k3d.io/) é uma ferramenta para executar o Kubernetes em containers [**Docker**](https://docs.docker.com/).

Como pré-requisito, você precisa ter o Docker devidamente instalado e funcional. [Clicando aqui](https://docs.docker.com/get-docker/) você será direcionado para a documentação de instalação do Docker.

**01. Download e instalação do k3d.**

Na página do [**Github**](https://github.com/k3d-io/k3d/releases) do projeto você encontra os arquivos compilados para diversas distribuições.

Exemplo de instalação em um GNU/Linux x86_64.

```bash
wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```
Verifique a versão do k3d.
```bash
k3d version
```

**02. Criando o cluster.**

É possível inicializar o k3d com apenas um nó contendo todas as funções necessárias do Kubernetes, e é o que faremos aqui.

Um exemplo prático disso é executar o comando abaixo que irá subir um único node.

```bash
k3d cluster create cl-onobrerodrigo --k3s-arg "--disable=traefik@server:*" --k3s-arg "--disable=servicelb@server:*"
```

- O cluster foi criado sem o trafik e sem o servicelb, pois iremos usar outros serviços no lugar.

Após criado, verifique o cluster e seu único nó.

```bash
k3d cluster list
```

Esse precisa ser o resultado:
```bash
NAME               SERVERS   AGENTS   LOADBALANCER
cl-onobrerodrigo   1/1       0/0      true
```
> Utilize o **help** do k3d para entender todas as possibilidades de uso.

Com o cluster inicializado, exporte a variável `KUBECONFIG`, pois no comando acima foi informado onde seria escrito o arquivo `kubeconfig file`.
```bash
export KUBECONFIG="~/.kube/k3d.yaml"
```

### Temos um cluster rodando localmente.