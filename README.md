# 📡 ntopng-network-monitoring-stack

A production-ready Docker-based network monitoring stack featuring **ntopng**, a secure **Nginx HTTPS reverse proxy**, and a persistent **Redis backend** to ensure stable configuration and prevent admin password resets. This setup supports reliable **NetFlow v9/IPFIX ingestion** from MikroTik routers or any compatible network device.

---

## 🚀 Overview

This project provides an isolated, maintainable, and scalable network monitoring environment built with Docker Compose. Its primary goal is to deliver a **reliable ntopng deployment** that:
- Keeps credentials and settings safe (Redis persistence)
- Supports HTTPS access securely (Nginx reverse proxy)
- Receives NetFlow/sFlow/IPFIX data without interruption
- Works consistently across restarts and container recreation

---

## 🧱 Architecture

The stack contains three core services:

### 🔹 ntopng  
The main monitoring and analytics engine responsible for processing network flow data.

### 🔹 Redis  
Stores ntopng settings and user credentials. Running Redis separately prevents the common issue of admin password resets.

### 🔹 Nginx  
Acts as an HTTPS reverse proxy to secure external access to ntopng.  
Handles:
- SSL termination  
- HTTP → HTTPS redirection  
- Clean routing to ntopng’s internal web UI  

---

## 🐳 Docker Composition

- Containers are isolated via a dedicated Docker network.  
- Volumes ensure persistent ntopng/Redis data.  
- Nginx exposes ports **80/443**.  
- ntopng exposes port **3000** for the UI.  

---

## 📁 Project Structure

/ntopng-network-monitoring-stack
├── docker-compose.yml
├── README.md
├── nginx/
│ ├── conf.d/ # Reverse proxy configs
│ └── certs/ # SSL certificates (ignored in Git)
└── volumes/
├── ntopng_data
├── ntopng_conf
└── redis_data


---

## ✨ Key Features

### 🔐 Secure HTTPS Access via Nginx
- SSL certificate support  
- HTTP → HTTPS redirection  
- Protects ntopng behind a proxy layer  

### 🛢 Persistent Redis Backend
- Stores ntopng users, settings, and runtime data  
- Ensures admin password never resets  

### 📡 NetFlow v9/IPFIX Collector
- Fully supports MikroTik NetFlow v9  
- Verified using tcpdump  
- Collector configured through ntopng UI  

### 🛠 Clean & Maintainable Architecture
- No deprecated CLI flags  
- Stability-first design  
- UI-based collector configuration  

---

## 🧩 Challenges & Solutions

### ✔ Preventing Admin Password Reset  
Dedicated Redis container with persistent volume.

### ✔ Avoiding Nginx 502 Errors  
Ensured ntopng starts cleanly by removing unsupported arguments.

### ✔ Ensuring NetFlow Reception  
Validated using tcpdump and configured collector in the ntopng UI.

### ✔ Solving Deprecated ntopng Flags  
Used UI instead of broken CLI flags.

---

## 🚀 Deployment

### Start the stack:
```bash
docker compose up -d
```
Stop the stack:

```bash
docker compose down
```

##📡 Configure NetFlow on MikroTik

```bash
/ip traffic-flow set enabled=yes
```

```bash
/ip traffic-flow target add address=SERVER_IP:2055 version=9
```

## 🛡 .gitignore Recommendations

```bash
nginx/certs/
*.pem
*.key
```

## 👨‍💻 Author

## Hamidreza Safarpour
Network & DevOps Engineer
GitHub: https://github.com/sphamid
