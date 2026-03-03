# 🐳 Nginx + Flask com Docker Compose

Projeto de demonstração de uma arquitetura com **Nginx como reverse proxy** para uma aplicação **Python/Flask**, orquestrado via **Docker Compose**.

## 🏗️ Arquitetura

```
                    ┌─────────────┐
      HTTP :80      │             │     HTTP :5000
  ───────────────►  │    Nginx    │  ───────────────►  Flask (Gunicorn)
                    │  (Proxy)    │
                    └─────────────┘
```

- **Nginx**: Recebe as requisições externas e faz o proxy reverso para o backend
- **Flask + Gunicorn**: Aplicação Python rodando em modo produção com 2 workers
- **Docker Network**: Comunicação interna isolada entre os containers

## 🚀 Como rodar

### Pré-requisitos
- Docker >= 24.x
- Docker Compose >= 2.x

### Subindo o ambiente

```bash
# Clonar o repositório
git clone https://github.com/seu-usuario/nginx-flask-docker.git
cd nginx-flask-docker

# Subir os containers
docker compose up -d --build

# Verificar status
docker compose ps
```

### Testando os endpoints

```bash
# Endpoint principal
curl http://localhost/

# Health check da aplicação
curl http://localhost/health

# Informações da app
curl http://localhost/info

# Health check do Nginx
curl http://localhost/nginx-health
```

### Visualizando logs

```bash
# Todos os serviços
docker compose logs -f

# Apenas o nginx
docker compose logs -f nginx

# Apenas o backend
docker compose logs -f backend
```

### Derrubando o ambiente

```bash
docker compose down
```

## 📁 Estrutura do projeto

```
.
├── backend/
│   ├── app.py            # Aplicação Flask
│   ├── requirements.txt  # Dependências Python
│   └── Dockerfile        # Imagem do backend
├── nginx/
│   └── nginx.conf        # Configuração do reverse proxy
├── docker-compose.yml    # Orquestração dos serviços
└── README.md
```

## 🔍 Conceitos demonstrados

- **Reverse Proxy** com Nginx
- **Multi-container** com Docker Compose
- **Health Check** nativo do Docker
- **Networks isoladas** entre serviços
- **Variáveis de ambiente** por container
- **Usuário não-root** no container (segurança)
- **Cache de layers** no Dockerfile (otimização de build)
- **Gunicorn** como servidor WSGI de produção

## 🛠️ Tecnologias

| Tecnologia | Versão | Papel |
|---|---|---|
| Nginx | 1.25 Alpine | Reverse Proxy |
| Python | 3.12 Slim | Runtime |
| Flask | 3.0.3 | Web Framework |
| Gunicorn | 22.0.0 | WSGI Server |
| Docker Compose | v2 | Orquestração |

---

**Autor:** Raphael Barbosa  
**LinkedIn:** [linkedin.com/in/seu-perfil](https://linkedin.com/in/seu-perfil)
