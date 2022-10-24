# 

# Depurar pods em execução
<img src="http://img.shields.io/static/v1?label=STATUS&message=EM%20DESENVOLVIMENTO&color=GREEN&style=for-the-badge"/>
</p>

## O que são Ephemeral Containers?

* Os contêineres efêmeros são um novo tipo de contêiner que faz parte da API principal do Kubernetes.
Um contêiner efêmero pode ser adicionado a um pod existente para ações administrativas como depuração,
ele é executado até sair e não será reiniciado. Um contêiner efêmero é executado na alocação de recursos 
existente do pod e compartilha namespaces de contêiner comuns. 

## Solução de problemas de clusters:

* Eu executo binário em uma imagem de contêiner sem distroless. De repente, um de seus pods está tendo 
problemas para se conectar a um serviço de back-end, mas como é uma imagem distroless, não posso usar
o kubectl exec para solucionar 


 * Podemos usar o kubectl debug para adicionar um contêiner efêmero e testar o serviço de back-end:


 
```markdown 
kubectl debug -it goapp --image=gcr.io/distroless/python3 --share-processes --copy-to=goapp-debug
```

# Crie uma imagens de contêiner "distroless"

* As imagens "distroless" contêm apenas seu aplicativo e suas dependências de tempo de execução. Eles não contêm gerenciadores de pacotes, shells ou quaisquer outros programas

```markdown 
# Start by building the application.
FROM golang:1.18 as build

WORKDIR /go/src/app
COPY . .

RUN go mod download
RUN CGO_ENABLED=0 go build -o /go/bin/app

# Now copy it into our base image.
FROM gcr.io/distroless/static-debian11
COPY --from=build /go/bin/app /
CMD ["/app"]
```
* Va até o diretório e execute

```markdown 
docker build -t myapp .
docker run -t myapp
```
