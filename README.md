# 🧅 n8n Tor Stack

A Docker Compose based self-hosted automation stack that routes **n8n outbound traffic through the Tor network** using Privoxy as an HTTP proxy gateway.

This project provides a portable and maintainable infrastructure for running automation workflows in environments where direct access to external services may be limited, filtered, or unreliable.

---

## ✨ Features

- 🐳 Fully containerized with Docker Compose
- 🧅 Tor-based outbound traffic routing
- 🌐 HTTP proxy support through Privoxy
- 🤖 n8n automation platform integration
- 🔒 Internal Docker networking
- 🚫 No hardcoded container IP addresses
- 💾 Persistent n8n data storage
- 🔧 Portable across Linux servers
- 📦 Infrastructure-as-code approach

---

# 🏗 Architecture

```
                         Internet
                            |
                            |
                       Tor Network
                            |
                            |
                    +---------------+
                    |      Tor      |
                    | SOCKS5 :9050  |
                    +-------+-------+
                            |
                            |
                    Docker Network
                            |
                            |
                    +---------------+
                    |    Privoxy    |
                    | HTTP  :8118   |
                    +-------+-------+
                            |
                            |
                    Docker Network
                            |
                            |
                    +---------------+
                    |      n8n      |
                    | Automation    |
                    |   :5678       |
                    +---------------+

```

---

# 🔄 Traffic Flow

When an n8n workflow sends an HTTP request:

```
n8n
 |
 | HTTP Proxy
 |
 v
Privoxy
 |
 | SOCKS5
 |
 v
Tor
 |
 |
Tor Network
 |
 |
External Service
```

The application does not directly communicate with the internet.

All outbound traffic is routed through:

```
n8n → Privoxy → Tor → Internet
```

---

# 🎯 Why Tor?

## 1. Network Accessibility and Censorship Resistance

In some regions, including environments with heavy internet filtering or network restrictions, access to external services and APIs can be limited.

Examples:

- External APIs
- RSS feeds
- Developer resources
- Community package repositories
- Automation services

Routing traffic through Tor can provide an alternative network path when direct connectivity is unavailable or restricted.

---

## 2. Free and Privacy-Oriented Infrastructure

Tor provides:

- A distributed volunteer-operated network
- Free access
- Encrypted communication between Tor nodes
- No dependency on commercial VPN providers

This makes it useful for privacy-focused networking experiments and self-hosted infrastructure.

---

# 🧩 Components

## Tor

Responsible for:

- Creating Tor circuits
- Providing SOCKS5 proxy service
- Routing outbound connections through the Tor network

Internal endpoint:

```
tor:9050
```

---

## Privoxy

Responsible for:

- Providing HTTP proxy compatibility
- Translating HTTP proxy requests into SOCKS5 requests
- Forwarding traffic to Tor

Internal endpoint:

```
privoxy:8118
```

---

## n8n

Responsible for:

- Workflow automation
- API integrations
- Scheduled jobs
- External service communication

Examples:

- Telegram bots
- RSS automation
- Web APIs
- Custom workflows

---

# 📁 Project Structure

```
n8n-tor-stack/

├── docker-compose.yml
│
├── tor/
│   ├── Dockerfile
│   └── torrc
│
├── privoxy/
│   ├── Dockerfile
│   └── config
│
├── n8n/
│   └── data/
│
├── .env.example
├── .gitignore
└── README.md

```

---

# 🚀 Deployment

## Requirements

- Linux server
- Docker Engine
- Docker Compose plugin

Example installation:

```bash
docker compose version
```

---

## Clone Repository

```bash
git clone https://github.com/USERNAME/n8n-tor-stack.git

cd n8n-tor-stack
```

---

## Configure Environment

Create your environment file:

```bash
cp .env.example .env
```

Edit variables if required.

---

## Start the Stack

Build images:

```bash
docker compose build
```

Start services:

```bash
docker compose up -d
```

Check status:

```bash
docker compose ps
```

---

# 🔍 Testing

## Check Tor

View logs:

```bash
docker compose logs tor
```

A successful startup should contain:

```
Bootstrapped 100% (done): Done
```

---

## Check Privoxy

```bash
docker compose logs privoxy
```

---

## Access n8n

Open:

```
http://localhost:5678
```

---

# 🛠 Debugging

## View all logs

```bash
docker compose logs -f
```

---

## Enter container shell

Example:

```bash
docker compose exec n8n sh
```

---

## Check Docker networks

```bash
docker network ls
```

---

## Inspect service communication

Example:

```bash
docker compose exec n8n ping privoxy
```

---

# 🔐 Security Considerations

## Internal-only proxy services

Tor and Privoxy are not exposed to the host network.

Communication happens only through the Docker internal network.

---

## No hardcoded IP addresses

Services communicate using Docker DNS:

```
privoxy:8118
tor:9050
```

instead of:

```
172.x.x.x
```

This keeps the stack portable.

---

## Secret Management

Sensitive values should never be committed.

Examples:

- API tokens
- passwords
- encryption keys

Use:

```
.env
```

and keep it outside version control.

---

# ⚠️ Limitations

Tor is designed for privacy and censorship resistance, not high-speed networking.

Expected:

- Higher latency
- Lower throughput
- Some services may block Tor exit nodes
- Some APIs may require additional configuration

---

# 🧪 Future Improvements

Possible improvements:

- HTTPS reverse proxy with Caddy/Nginx
- Automatic Tor health checks
- Container healthchecks
- n8n PostgreSQL backend
- Secret management using Docker secrets
- Monitoring with Prometheus/Grafana
- Automated deployment pipeline

---

# 📝 License

This project is provided for educational and infrastructure development purposes.

Use responsibly and comply with applicable laws and service terms.
