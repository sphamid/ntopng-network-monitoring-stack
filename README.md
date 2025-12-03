# ntopng-network-monitoring-stack

📡 ntopng-network-monitoring-stack

A production-ready, Docker-based network monitoring stack featuring ntopng, a secure Nginx HTTPS reverse proxy, and a persistent Redis backend to prevent configuration and password resets.
This project is designed to reliably collect and analyze NetFlow v9 / IPFIX data from MikroTik routers or any compatible network device.

🚀 Overview

This repository provides a fully isolated, containerized monitoring environment built around ntopng.
The architecture focuses on:

Security (HTTPS reverse proxy)

Persistence (Redis for stable configuration & credentials)

Reliability (NetFlow ingestion)

Portability (Docker-based stack)

Clean separation of components

The stack is production-grade, easy to deploy, and suitable for real operational use.

🧱 Architecture

The stack consists of three core services:

🔹 1. ntopng (Network Monitoring Engine)

Runs as the main analytics system.
Processes NetFlow/IPFIX packets and provides a web dashboard.

🔹 2. Redis (Persistent Database)

Stores:

User accounts (including admin)

Application settings

Internal ntopng runtime data

Running Redis externally prevents the common issue of admin password reset when the ntopng container is recreated.

🔹 3. Nginx (HTTPS Reverse Proxy)

Provides:

Secure access through SSL (HTTPS)

HTTP→HTTPS redirection

Clean routing to ntopng’s web interface

Isolation between frontend entry point and backend monitoring engine

🐳 Docker Composition Summary

docker-compose.yml defines all services

Volumes ensure no data loss

Nginx exposes ports 80/443

ntopng exposes 3000 (UI) and optionally 2055/udp (NetFlow Collector)

Redis exposes its internal storage only through Docker network

🔐 Security Features

HTTPS enabled via Nginx

Certificates stored outside the repository (nginx/certs/)

Reverse proxy blocks direct ntopng exposure

Redis isolated inside Docker network

📡 NetFlow / IPFIX Support

This system can ingest network flow data such as:

NetFlow v9

IPFIX

sFlow (optional)

Typical use case:
MikroTik RouterOS exporting NetFlow v9 to the ntopng collector.

⚙️ Key Functionalities

Fully automated deployment with Docker Compose

Persistent ntopng configuration and credentials

Zero password resets due to Redis isolation

Secure HTTPS interface through Nginx

Flow ingestion validated using tcpdump

Clean logging and stable behavior even during restarts

🧩 Challenges & Solutions
✔ 1. ntopng admin password resetting

Cause: Redis data being lost or ntopng reinitialization
Solution: Dedicated Redis container with persistent volume

✔ 2. 502 Bad Gateway on Nginx

Cause: ntopng failing to start due to wrong CLI arguments
Solution: Minimal configuration and stable startup sequence

✔ 3. NetFlow packets reaching Linux but not ntopng

Cause: Collector not configured correctly inside ntopng
Solution: Activating collector through ntopng UI instead of CLI arguments

✔ 4. Deprecated ntopng flags

Cause: Outdated tutorials and broken parameters
Solution: Moving to UI-based collector configuration and minimal --redis argument only

📂 Project Structure
/
┣━ docker-compose.yml
┣━ README.md
┣━ nginx/
┃   ┣━ conf.d/         # Reverse proxy config
┃   ┗━ certs/          # SSL certificates (ignored in Git)
┗━ volumes:
    ┣━ ntopng_data
    ┣━ ntopng_conf
    ┗━ redis_data

    
🚀 Deployment

Start the entire monitoring stack:

docker compose up -d

Stop:

docker compose down

📡 Configure NetFlow on ntopng

/ip traffic-flow
set enabled=yes

/ip traffic-flow target
add address=SERVER_IP:2055 version=9

🛡 .gitignore Recommendations

nginx/certs/
*.pem
*.key

## 🧑‍💻 Author
**Hamidreza Safarpour**  
Network & DevOps Engineer  
GitHub: https://github.com/sphamid

📄 License

This project is open-source and can be adapted for production or educational use.


