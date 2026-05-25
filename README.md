# 🐳 Nginx + Flask with Docker Compose

A demonstration project of a multi-container architecture using **Nginx as a reverse proxy** for a Python/Flask application, orchestrated with Docker Compose.

---

## 🏗️ Architecture

```
                   ┌─────────────┐
     HTTP :80      │             │    HTTP :5000
 ──────────────►   │    Nginx    │ ──────────────►  Flask (Gunicorn)
                   │   (Proxy)   │
                   └─────────────┘
```

| Component | Role |
|---|---|
| **Nginx** | Receives external requests and forwards them to the backend via reverse proxy |
| **Flask + Gunicorn** | Python application running in production mode with 2 workers |
| **Docker Network** | Isolated internal communication between containers (bridge) |

---

## 🚀 Getting Started

### Prerequisites

- Docker >= 24.x
- Docker Compose >= 2.x

### Running the project

```bash
# Clone the repository
git clone https://github.com/raphadevops/nginx-flask-docker.git
cd nginx-flask-docker

# Build and start the containers
docker compose up -d --build

# Check running services
docker compose ps
```

---

## 🔗 Available Endpoints

| Endpoint | Description |
|---|---|
| `GET /` | Returns app status, container hostname and environment |
| `GET /health` | Application health check — returns `{ "status": "healthy" }` |
| `GET /info` | App metadata: name, version, author and stack |
| `GET /nginx-health` | Nginx health check — responds directly without hitting Flask |

### Testing

```bash
# Main endpoint
curl http://localhost/

# Application health check
curl http://localhost/health

# App info
curl http://localhost/info

# Nginx health check
curl http://localhost/nginx-health
```

---

## 📋 Useful Commands

```bash
# View logs from all services
docker compose logs -f

# View logs from a specific service
docker compose logs -f nginx
docker compose logs -f backend

# Stop and remove containers
docker compose down
```

---

## 📁 Project Structure

```
nginx-flask-docker/
├── backend/
│   ├── app.py              # Flask application with 3 routes
│   ├── requirements.txt    # Python dependencies (Flask + Gunicorn)
│   └── Dockerfile          # Backend image — python:3.12-slim
├── nginx/
│   └── nginx.conf          # Reverse proxy configuration
├── docker-compose.yml      # Services orchestration
└── README.md
```

---

## 🔍 Key Concepts Demonstrated

- **Reverse Proxy** — Nginx forwards all traffic to Flask internally
- **Multi-container orchestration** — Docker Compose managing two services
- **Native health check** — Docker monitors Flask before starting Nginx (`depends_on: condition: service_healthy`)
- **Isolated network** — containers communicate by service name, not IP (`app_network` bridge)
- **Environment variables** — `ENVIRONMENT=production` injected via Compose
- **Non-root user** — Flask runs as `appuser` inside the container (security best practice)
- **Dockerfile layer caching** — `requirements.txt` copied before `app.py` to avoid reinstalling dependencies on every code change
- **Gunicorn as WSGI server** — production-grade server with 2 workers replacing Flask's built-in development server

---

## 🛠️ Tech Stack

| Technology | Version | Role |
|---|---|---|
| Nginx | 1.25 Alpine | Reverse Proxy |
| Python | 3.12 Slim | Runtime |
| Flask | 3.0.3 | Web Framework |
| Gunicorn | 22.0.0 | WSGI Server |
| Docker Compose | v2 | Orchestration |

---

## 👤 Author

**Raphael** — [raphadevops](https://github.com/raphadevops)
