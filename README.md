# 🐳 Nginx + Flask com Docker Compose

Projeto de demonstração de uma arquitetura multi-container usando **Nginx como reverse proxy** para uma aplicação Python/Flask, orquestrada com Docker Compose.

---

## 🏗️ Arquitetura

```
                   ┌─────────────┐
     HTTP :80      │             │    HTTP :5000
 ──────────────►   │    Nginx    │ ──────────────►  Flask (Gunicorn)
                   │   (Proxy)   │
                   └─────────────┘
```

| Componente | Função |
|---|---|
| **Nginx** | Recebe as requisições externas e as repassa para o backend via reverse proxy |
| **Flask + Gunicorn** | Aplicação Python rodando em modo produção com 2 workers |
| **Docker Network** | Comunicação interna isolada entre containers (bridge) |

---

## 🔄 Próximo Passo Nesta Série

Este projeto demonstra orquestração em host único com Docker Compose. Para a evolução natural — orquestração multi-node com Docker Swarm, balanceamento de carga entre réplicas, banco de dados persistente, gerenciamento de secrets e rolling update com zero downtime — confira:

**[nginx-flask-swarm](https://github.com/raphadevops/nginx-flask-swarm)**

---

## 🚀 Como Executar

### Pré-requisitos

- Docker >= 24.x
- Docker Compose >= 2.x

### Executando o projeto

```bash
# Clonar o repositório
git clone https://github.com/raphadevops/nginx-flask-docker.git
cd nginx-flask-docker

# Construir e iniciar os containers
docker compose up -d --build

# Verificar os serviços em execução
docker compose ps
```

---

## 🔗 Endpoints Disponíveis

| Endpoint | Descrição |
|---|---|
| `GET /` | Retorna status da aplicação, hostname do container e ambiente |
| `GET /health` | Health check da aplicação — retorna `{ "status": "healthy" }` |
| `GET /info` | Metadados da aplicação: nome, versão, autor e stack |
| `GET /nginx-health` | Health check do Nginx — responde diretamente, sem passar pelo Flask |

### Testando

```bash
# Endpoint principal
curl http://localhost/

# Health check da aplicação
curl http://localhost/health

# Informações da aplicação
curl http://localhost/info

# Health check do Nginx
curl http://localhost/nginx-health
```

---

## 📋 Comandos Úteis

```bash
# Ver logs de todos os serviços
docker compose logs -f

# Ver logs de um serviço específico
docker compose logs -f nginx
docker compose logs -f backend

# Parar e remover os containers
docker compose down
```

---

## 📁 Estrutura do Projeto

```
nginx-flask-docker/
├── backend/
│   ├── app.py              # Aplicação Flask com 3 rotas
│   ├── requirements.txt    # Dependências Python (Flask + Gunicorn)
│   ├── Dockerfile          # Imagem do backend — python:3.12-slim
│   └── .dockerignore       # Exclui arquivos desnecessários do build
├── nginx/
│   └── nginx.conf          # Configuração do reverse proxy
├── docker-compose.yml      # Orquestração dos serviços
└── README.md
```

---

## 🔍 Conceitos-Chave Demonstrados

- **Reverse Proxy** — Nginx repassa todo o tráfego para o Flask internamente
- **Orquestração multi-container** — Docker Compose gerenciando dois serviços
- **Health check nativo** — Docker monitora o Flask antes de subir o Nginx (`depends_on: condition: service_healthy`)
- **Rede isolada** — containers se comunicam pelo nome do serviço, não por IP (`app_network` bridge)
- **Variáveis de ambiente** — `ENVIRONMENT=production` injetada via Compose
- **Usuário non-root** — Flask roda como `appuser` dentro do container (boa prática de segurança)
- **Cache de camadas do Dockerfile** — `requirements.txt` copiado antes de `app.py` para evitar reinstalar dependências a cada mudança de código
- **Gunicorn como servidor WSGI** — servidor de produção com 2 workers, substituindo o servidor de desenvolvimento nativo do Flask
- **Limites de recursos** — `deploy.resources.limits` define teto de memória por serviço

---

## 🛠️ Stack Tecnológica

| Tecnologia | Versão | Função |
|---|---|---|
| Nginx | 1.25 Alpine | Reverse Proxy |
| Python | 3.12 Slim | Runtime |
| Flask | 3.0.3 | Web Framework |
| Gunicorn | 22.0.0 | Servidor WSGI |
| Docker Compose | v2 | Orquestração |

---

## 👤 Autor

**Raphael** — [raphadevops](https://github.com/raphadevops)
