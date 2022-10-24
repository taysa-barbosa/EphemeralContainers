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
 
```markdown 
kubectl run ephemeral-demo --image=registry.k8s.io/pause:3.1 --restart=Never
```
Se você tentar usar o kubectl exec para criar um shell, verá um erro porque não há shell nesta imagem de contêiner.

```markdown 
kubectl exec -it ephemeral-demo -- sh
```

Em vez disso, você pode adicionar um contêiner de depuração usando kubectl debug.

```markdown 
kubectl debug ephemeral-demo -it --image=ubuntu --share-processes --copy-to=ephemeral-demo-debug
```



[^1]: [Ephemeral Containers](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/).
[^2]: [Debug Running Pods](https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/#ephemeral-container).
 
