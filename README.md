# Container Week Annotations

https://www.containerweek.com/

## 08/04/19 - Palestra com Jérome Petazzoni

- Introdução ao Kubernetes
- Como subir um cluster do Kubernetes sem Node

@jpetazzo

YouTube

https://www.youtube.com/watch?v=WkM9R9-WQCk

Slides

https://docs.google.com/presentation/d/1uJsRss15tYR-oed9N5BYqGLYEnj7RFgxgFyu_k3EQqQ/edit?usp=drivesdk

Live Demo

https://github.com/jpetazzo/container.training


## 09/04/19

- Docker
- Docker Compose
- Docker Swarm
- Deploy com Docker Stack

YouTube

https://www.youtube.com/watch?v=T3lpAjOcbU8

CheatSheet sobre Docker da LINUXtips

https://www.linuxtips.io/cheatsheet


iptables - comando pra conversar com módulo do kernel


Instalando o Docker

```
curl -fsSL https://get.docker.com | bash

docker version

docker container ls

docker container run hello-world
```

Interativo

```
docker container run -ti debian
```

já entra direto no container

Quando você sai do container com `exit`, ele morre.

```
docker container ls -a
```

Pra não matar o container tem que sair com

```
CTRL + p + q
```

Pra voltar para o container

```
docker container attach CONTAINER_ID ou NAMES
```

Rodando em segundo plano, modo daemon

```
docker container run -d -p 8080:80 nginx # imagem
docker image ls
docker container ls
```

Conectar sem usar o `attach`

```
docker container exec -ti CONTAINER_ID bash
```

```
cd /usr/share/nginx/html/
echo "GIROPOPS STRIGUS GIRUS" > index.html
```

Deletar o container

```
docker container ls
docker container rm -f CONTAINER_ID
```

## Subir cluster com Swarm

Orquestração dos container

https://docs.docker.com/engine/swarm/


* Alta disponibilidade
* Balanceamento de carga
* Escalável

```
docker swarm init
```

Como adicionar outro nó no cluster

```
docker swarm join --token ID porta:2377
docker node ls # mostra todos os nós do cluster
```

manager - é o gerenciador do cluster

ou

worker - somente executa os container


Se você tiver vários manager, você pode perder somente 1. Ou seja, você deve manter mais que 51% dos manager dentro do cluster. Mais do que isso você perder o cluster.


```
docker swarm join-token manager
docker swarm join-token worker
```

Vai no outro nó e digite

```
docker swarm join --token TOKEN porta:2377
```

```
docker node promote HOSTNAME # vira manager
docker node demote HOSTNAME # vira worker
```

```
docker node inspect HOSTNAME
```

## Criando um service

Criar e expor o container

```
docker service create --name nginx -p 8080:80 --replicas 5 nginx
```

ou `--publish`

```
docker service ls
```

Aonde estão rodando os container

```
docker service ps nginx
```

Para retornar a página

```
curl 0:8080
```

Logs

```
docker service logs -f nginx
```

```
docker service create --name nginx -p 8080:80 --replicas 1 --limit-cpu 0.2 --limit-memory 64M nginx
```

Ver o consumo

```
docker container stats ID
```

Escalando

```
docker service scale nginx=10
```

Criando mais um serviço

```
docker service create --name giropops -p 8080:80 linuxtips/nginx-prometheus-exporter:1.0.0
curl 0:8080
docker service scale giropops=10
docker service ls
```

Update

```
docker service update --image linuxtips/nginx-prometheus-exporter:2.0.0 giropops
curl ip:porta
```

https://github.com/badtuxx/giropops-monitoring

Docker Compose é para criar vários serviços

## Instalando rexray

https://rexray.io/

Storages suportados pelo RexRay

https://rexray.readthedocs.io/en/v0.10.0/user-guide/storage-providers/

```
curl -sSL https://rexray.io/install | sh -s -- stable
```

```
vim /etc/
systemctl restart rexray
systemctl status rexray
```

Compose file, usar o volume

Criando volume

```
docker volume create --driver rexray grafana_d
```

```
docker volume ls
```

Editando mais alguns arquivos

```
vim conf/alertmanager/config.yml
vim conf/promotheus/promotheus.yml
```

```
rexray volume mount promotheus_c
df -h
```

```
cp conf/promotheus/* /var/lib/rexray/volumes/promotheus_c/data/
```

No final tem que desmontar o volume.

```
docker stack deploy -c docker-compose.yml giropops
docker stack ls
docker stack ps
```



## 10/04/19

- Kubernetes
- Componentes
- Criar cluster do Kubernetes
- Deploy num cluster do Kubernetes
- Helm by Kubernetes

```
curl -fsSL https://get.docker.com | bash

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/k8s ...

apt-get install kubelet kubeadm kubectl

# desabilitar swap

swapoff -a

docker info

docker info | grep cgroup

vim /etc/docker/daemon.json

conteúdo na doc do kubernetes

https://kubernetes.io/docs/home/

mkdir -p /etc/systemd/system/docker.service.d

systemctl daemon-reload

systemctl restart docker

docker info | grep -i cgroup

kubeadm config images pull

kubeadm init

# Criar as configurações conforme indicado no terminal

kubectl apply -f "url"

kubectl get pods -n kube-system

kubectl get nodes

kubectl describe node nome_da_maquina

echo "source <(kubectl completion bash)" >> ~/.bashrc

kubeadm token create --print-join-command

kubectl get namespaces

kubectl run nginx --image-nginx

kubectl get deployments

kubectl get replicaments

kubectl describe replicasets. name

kubectl describe pods
```


### Criar service

```
kubectl expose deployment nginx

kubectl expose deployments. nginx

kubectl run nginx --port 80 --image=nginx

kubectl PERDI

kubectl expose deployment nginx

kubectl get service

kubectl expose deployment nginx --type=NodePort

curl 0:31717 # porta gerada randomic

kubectl edit service nginx

kubectl expose deployment nginx --type=LoadBalancer

kubectl get service
```


Projeto com k8s usado como exemplo

https://github.com/badtuxx/k8s-canary-deploy-example


Instalação do Kubernetes

https://kubernetes.io/docs/tasks/tools/install-kubectl/


Doc do Helm

https://helm.sh/docs/













## 11/04/19

- Service Mesh
- Istio - controlar micro serviços


## 12/04/19

- Certificações Docker e Kubernetes
    - Docker
    - Linux Foundation



## Rodando instâncias online

https://labs.play-with-docker.com/

https://labs.play-with-k8s.com/

