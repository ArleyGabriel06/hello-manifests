# MANIFESTOS

## 📄 Estrutura do deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-fastapi
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-fastapi
  template:
    metadata:
      labels:
        app: hello-fastapi
    spec:
      containers:
      - name: hello-fastapi
        image: gabriel6garcia/hello-fastapi:3304559e46c3cb6a3fbd7e264fe182d13d1278aa
        ports:
          - containerPort: 8081
```

| Campo                      | Descrição                                                         |
| -------------------------- | ----------------------------------------------------------------- |
| `apiVersion: apps/v1`      | Usa a versão da API do Kubernetes para `Deployment`.              |
| `kind: Deployment`         | Especifica que estamos criando um recurso do tipo Deployment.     |
| `metadata.name`            | Nome do Deployment: `hello-fastapi`.                              |
| `spec.replicas`            | Define o número de réplicas do pod (neste caso, 1).               |
| `selector.matchLabels`     | O Deployment irá gerenciar pods com o label `app: hello-fastapi`. |
| `template.metadata.labels` | Aplica o label `app: hello-fastapi` aos pods criados.             |
| `spec.containers`          | Lista de containers que o pod deve rodar.                         |
| `name: hello-fastapi`      | Nome do container.                                                |
| `image`                    | Imagem Docker hospedada no Docker Hub, identificada por seu hash. |
| `ports.containerPort`      | Expõe a porta `8081` do container, usada pelo servidor FastAPI.   |

## 📄 Estrutura do service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-fastapi-service
spec:
  type: NodePort
  selector:
    app: hello-fastapi
  ports:
  - port: 80
    targetPort: 8081
    nodePort: 30081
```

| Campo                         | Descrição                                                                                   |
| ----------------------------- | ------------------------------------------------------------------------------------------- |
| `apiVersion: v1`              | Usa a versão da API do Kubernetes para Services.                                            |
| `kind: Service`               | Define que este recurso é um Service.                                                       |
| `metadata.name`               | Nome do Service: `hello-fastapi-service`.                                                   |
| `spec.type: NodePort`         | Expõe a aplicação fora do cluster, usando a porta de um nó. Ideal para testes locais.       |
| `selector.app: hello-fastapi` | Faz o Service apontar para os pods com o label `app: hello-fastapi`.                        |
| `ports.port`                  | Porta interna do cluster (o serviço escutará na porta 80 dentro do cluster).                |
| `ports.targetPort`            | Porta do container onde o FastAPI está rodando (`8081`).                                    |
| `ports.nodePort`              | Porta externa do nó (máquina) que será usada para acessar a aplicação. Neste caso, `30081`. |
