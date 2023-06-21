##### [Instalação do *kind*]():
```
[ $(uname -m) = x86_64 ] && curl -Lo ./kind \
https://kind.sigs.k8s.io/dl/v0.19.0/kind-linux-amd64
```

##### [Configuração do *kind*]():
```
$ sudo mv -f kind /usr/local/bin/kind
$ sudo chmod +x /usr/local/bin/kind
```

##### Exibe a versão instalada do *kind*:
`$ kind version`

##### Criação do *cluster* com *kind*:

```
# This file: cluster-config.yml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
# One control plane node and three "workers".
#
# While these will not add more real compute capacity and
# have limited isolation, this can be useful for testing
# rolling updates etc.
#
# The API-server and other control plane components will be
# on the control-plane node.
#
# You probably don't need this unless you are testing Kubernetes itself.
nodes:
- role: control-plane
- role: worker
- role: worker
- role: worker
```

##### Cria um *cluster* (com o nome balerion) com base num arquivo de configuração:
`$ kind create cluster --config cluster-config.yml --name balerion`

##### Exibe os detalhes do *cluster*:
`$ kubectl get pods -A`

##### Exibe a ajuda do comando `kind create cluster`:
`$ kind create cluster --help`

##### [Instalação do *kubectl*]():

```
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s \
https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
$ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

#####
`$ kubectl cluster-info --context kind-kind`

##### Exibe todas as informações a respeito do *cluster* criado:
`$ kubectl cluster-info dump`

##### Exibe todos os *pods* em detalhes (em todos os *namespaces*):
`$ kubectl get pods -A -owide` ou `$ kubectl get pod -A -o wide`

##### Exibe os *pods* de um *namespace* específico, formatando a saída em `YAML`:
`$ kubectl get pods -n kube-system -o yaml`

##### Exibe os namespaces disponíveis:
`$ kubectl get namespaces`

##### Exibe todos os *nodes* do *cluster* *Kubernetes*:
`$ kubectl get node`

##### Exibe todos os *nodes* do *cluster* *Kubernetes* de forma detalhada:
`$ kubectl get node -owide`
`$ kubectl get node -o wide`

##### Exibe todos os *nodes* do *cluster* do com *debug* muito detalhado:
`$ kubectl get node -v9`

##### Destrói um *cluster* criado com o *kind*:
`$ kind delete cluster`

##### Exibe os detalhes dos *nodes* do *cluster*:
`$ kubectl describe nodes`

##### Exibe os detalhes de um *pod*:
`$ kubectl describe pod <pod_name>`

##### Exibe os detalhes de um *pod* de determinado *namespace*:
`$ kubectl describe pods -n kube-system kube-proxy-6ncxl`

##### Cria um *pod* com base numa imagem do *nginx*:
`$ kubectl run --image nginx nginx`

##### Cria um arquivo base para um *pod*, gravando este template no arquivo `pod.yml`:
`$ kubectl run --image nginx --dry-run -oyaml nginx > pod.yml`

##### Cria um arquivo base para um *deployment*, gravando este *template* no arquivo `deployment.yml`:
```
$ kubectl create deployment strigus \
--image nginx --replicas 12 \
-n giropops --port 80 \
--dry-run=client -o yaml > deployment.yml
```

##### Cria um *resource* do tipo *deployment* com base em um arquivo `YAML`:
`$ kubectl apply -f deployment.yml`

##### Cria um *resource* do tipo *pod* com base em um arquivo `YAML`:
`$ kubectl apply -f pod.yml`

##### Remove um *pod* de nome nginx:
`$ kubectl delete pod nginx`

##### Cria um *deployment* (strigus) num *namespace* específico (giropops):
`$ kubectl create deployment strigus --image nginx --replicas 20 -n giropops`

##### Exibe os *deployments* disponíveis:
`$ kubectl get deployment`

##### Exibe os detalhes de um *deployment*:
`$ kubectl get deployment -owide` ou `$ kubectl get deployment -o wide`

##### Expõe um *pod*, criando um *service* do tipo ClusterIP (somente acessível a partir do *cluster*):
`$ kubectl expose pod nginx`

##### Expõe um *pod*, criando um *service* do tipo NodePort (acessível também externamente):
`$ kubectl expose pod nginx --type NodePort`

##### Expõe um *pod*, criando um *service* do tipo LoadBalancer:
`$ kubectl expose pod nginx --type LoadBalancer`

##### Exibe os *logs* de um *service*:
`$ kubectl logs nginx`

##### Exibe os *logs* de um *service* e continua acompanhando (como no `tail -f`):
`$ kubectl logs -f nginx`

##### Interage, via terminal, com um *pod*:
`$ kubectl exec -ti nginx bash`

##### Cria um *namespace*:
`$ kubectl create namespace giropops`

##### Exibe os *namespaces* disponíveis:
`$ kubectl get namespaces`
`$ kubectl get ns`

##### Cria um *deployment* (strigus) em um *namespace* determinado (giropops) com a porta 80: 
`$ kubectl create deployment strigus --image nginx --replicas 20 -n giropops --port 80`
> A menos que seja declarado de forma explícita, um *resource* geralmente será criado no *namespace* *default*.

##### Exibe detalhes de um *namespace*:
`$ kubectl describe namespace giropops`

##### Exibe os *pods* de determinado *namespace* (giropops):
`$ kubectl get pod -n giropops`

##### Expõe um *deployment*, como *service* do tipo **ClusterIP** no *namespace* giropops:
`$ kubectl expose deployment -n giropops strigus`
> Geralmente expomos como *service* *pods* ou *deployments*.

##### Exibe os *services* de determinado *namespace*:
`$ kubectl get service -n giropops`

##### Exibe os detalhes de um *service* (strigus) em determinado *namespace*:
`$ kubectl describe service strigus -n giropops`

##### Exibe os *replicaset* de um *deployment* em determinado *namespace*:
`$ kubectl get replicaset -n giropops`

##### Exibe os detalhes de um *replicaset* (strigus-c8b68cff7) em determinado *namespace*:
`$ kubectl describe replicaset strigus-c8b68cff7 -n giropops`

##### Exibe somente os nomes dos *pods* em determinado *namespace*:
`$ kubectl get pod --no-headers=true -n giropops | cut -d ' ' -f 1`
> Útil para remover todos os *pods* de uma única vez.

##### Remove, de uma só vez, todos os *pods* em determinado *namespace*:
`$ kubectl delete pod -n giropops $(kubectl get pod --no-headers=true -n giropops | cut -d " " -f 1`)

##### Exibe ajuda para os *resources* do tipo *pod* do *Kubernetes*:
`$ kubectl explain pods`

##### Exibe todos os *endpoints* de um *service* em determinado *namespace*:
`$ kubectl get endpoints strigus -n giropops`
> Quando criamos um *service*, temos associado a ele um *endpoint* que conhece os endereços de todos os *pods* daquele *service*.

##### Exibe todos os *endpoints* em determinado *namespace*:
`$ kubectl get endpoints -n giropops`

##### Exibe todos os *endpoints* em determinado *namespace*, de forma detalhada:
`$ kubectl describe endpoints -n giropops`

##### Exibe todos os *endpoints* de determinado *service* (strigus) em determinado *namespace*, de forma detalhada:
`$ kubectl describe endpoints strigus -n giropops`
