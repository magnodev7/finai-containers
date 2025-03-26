# FinAI Container Registry

Este repositório contém as configurações e instruções para o registro de containers da aplicação FinAI.

## Configuração do GitHub Container Registry

### 1. Habilitar o GitHub Container Registry

1. Vá para Settings > Packages
2. Habilite "Improved container support"
3. Configure a visibilidade dos pacotes como "Public"

### 2. Configurar Autenticação

```bash
# Login no GitHub Container Registry
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin
```

### 3. Build e Push das Imagens

```bash
# Build e push da imagem frontend
docker build -t ghcr.io/magnodev7/finai-frontend:latest -f Dockerfile.frontend .
docker push ghcr.io/magnodev7/finai-frontend:latest

# Build e push da imagem backend
docker build -t ghcr.io/magnodev7/finai-backend:latest -f Dockerfile.backend ./backend
docker push ghcr.io/magnodev7/finai-backend:latest
```

### 4. Configuração do Kubernetes

Atualize os manifestos do Kubernetes para usar as novas imagens:

```yaml
# frontend-deployment.yaml
spec:
  template:
    spec:
      containers:
      - name: finai-frontend
        image: ghcr.io/magnodev7/finai-frontend:latest

# backend-deployment.yaml
spec:
  template:
    spec:
      containers:
      - name: finai-backend
        image: ghcr.io/magnodev7/finai-backend:latest
```

### 5. Configurar Secret no Kubernetes

```bash
# Criar secret para autenticação no GitHub Container Registry
kubectl create secret docker-registry ghcr-secret \
  --docker-server=ghcr.io \
  --docker-username=magnodev7 \
  --docker-password=$GITHUB_TOKEN \
  --namespace=finai
```

## Imagens Disponíveis

- Frontend: `ghcr.io/magnodev7/finai-frontend:latest`
- Backend: `ghcr.io/magnodev7/finai-backend:latest`