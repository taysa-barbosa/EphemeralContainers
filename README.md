# 

# Depurar pods em execução
<img src="http://img.shields.io/static/v1?label=STATUS&message=EM%20DESENVOLVIMENTO&color=GREEN&style=for-the-badge"/>
</p>

## O que são Ephemeral Containers?[^1]

* Os contêineres efêmeros são um novo tipo de contêiner que faz parte da API principal do Kubernetes.
Um contêiner efêmero pode ser adicionado a um pod existente para ações administrativas como depuração,
ele é executado até sair e não será reiniciado. Um contêiner efêmero é executado na alocação de recursos 
existente do pod e compartilha namespaces de contêiner comuns. 

## Solução de problemas de clusters:

* Eu executo binário em uma imagem de contêiner sem distroless. De repente, um de seus pods está tendo 
problemas para se conectar a um serviço de back-end, mas como é uma imagem distroless, não posso usar
o kubectl exec para solucionar 


 * Podemos usar o kubectl debug para adicionar um contêiner efêmero e testar o serviço de back-end:


# Exemplo de depuração usando contêineres efêmeros [^2]

 Exemplo de imagem que não contém utilitários de depuração, rode o seguinte comando
 
```
kubectl run ephemeral-demo --image=registry.k8s.io/pause:3.1 --restart=Never
```
Se você tentar usar o kubectl exec para criar um shell, verá um erro porque não há shell nesta imagem de contêiner.

```
kubectl exec -it ephemeral-demo -- sh
```

Execute este comando para criar uma cópia de ephemeral-demo chamada ephemeral-demo-debug que adiciona um novo contêiner do Ubuntu para depuração:

``` 
kubectl debug ephemeral-demo -it --image=ubuntu --share-processes --copy-to=ephemeral-demo-debug
```
*O --share-processes permite que os contêineres neste pod vejam os processos de outros contêineres no pod*. 


Você pode visualizar o estado do contêiner efêmero recém-criado usando:

```
kubectl describe pod ephemeral-demo
```
Use kubectl delete para remover o pod quando terminar:

```
kubectl delete pod ephemeral-demo
```

# Considerações 

Os contêineres efêmeros são úteis quando o kubectl exec é insuficiente porque um contêiner travou ou uma imagem de contêiner não inclui utilitários de depuração.

Em particular, as imagens distroless permitem que você implante imagens de contêiner mínimas que reduzem a superfície de ataque e a exposição a bugs e vulnerabilidades. Como as imagens distroless não incluem um shell ou utilitários de depuração, é difícil solucionar problemas de imagens distroless usando apenas o kubectl exec.

[^1]: [Ephemeral Containers](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/).
[^2]: [Debug Running Pods](https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/#ephemeral-container).


