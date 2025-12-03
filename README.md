# ntopng-network-monitoring-stack

Overview

This project provides a fully modular, production-ready Docker environment for running ntopng, complete with a secure HTTPS reverse proxy, persistent configuration storage, and reliable NetFlow ingestion from MikroTik routers.

The goal was to build a stable, maintainable, and resilient network monitoring stack that avoids common issues such as admin password resets, configuration loss, and unstable collector behavior.

Key Features
🔐 Secure Nginx Reverse Proxy (HTTPS)

An Nginx container sits in front of ntopng to provide:

Full HTTPS support using SSL certificates

HTTP → HTTPS redirection

A clean and secure interface for remote access

Improved separation between frontend access and backend services

This makes the stack suitable for real production environments.

🛢 Persistent Redis Backend (Avoiding Password Reset)

ntopng stores user credentials and critical settings in Redis.
To prevent losing admin passwords during container recreation:

Redis runs as an independent container

Uses its own persistent Docker volume

Ensures ntopng never resets credentials again

This architecture guarantees long-term stability and eliminates the common “admin password resets after restart” problem.

📡 NetFlow v9 Collector for MikroTik

The stack is configured to ingest network flow data from MikroTik routers:

UDP port 2055 exposed to the host

Flows verified using tcpdump

Collector added through ntopng’s Web UI

Fully compatible with NetFlow v9/IPFIX

This makes the environment an ideal flow analytics solution for small and medium network setups.

📦 Clean and Maintainable Docker Setup

The project includes:

docker-compose.yml

Isolated services (ntopng, Redis, Nginx)

Proper Docker networks and volumes

No custom scripts or hacks

Fully portable and reproducible

Challenges & Solutions
1. Admin Password Resetting

Problem:
ntopng resets the admin password whenever Redis data is lost or misconfigured.

Solution:
Dedicated Redis container + persistent volume → password stays permanently.

2. ntopng Not Receiving NetFlow

Problem:
Packets reached the Linux host but didn’t appear inside ntopng.

Solution:
Collector activated via ntopng Web UI, correct UDP port exposed, and packet flow verified using tcpdump.

3. Nginx 502 Errors

Problem:
Reverse proxy could not connect to ntopng because ntopng didn’t fully start due to misconfiguration.

Solution:
Removed incorrect ntopng flags and used a minimal configuration, ensuring ntopng booted cleanly.

4. Incompatible ntopng CLI Arguments

Problem:
Many older tutorials and arguments like --collector-port caused crashes.

Solution:
Shifted to ntopng’s internal UI-based collector configuration and removed deprecated arguments.

Final Result

A clean, stable, secure, and production-ready ntopng Docker environment that includes:

HTTPS protected access

Persistent configuration

Reliable NetFlow ingestion

Modular containerized architecture

No crashes, no password resets

Easy to migrate, backup, and redeploy

This project also serves as a portfolio-quality demonstration of Docker, Nginx reverse proxying, Redis management, network flow handling, and DevOps troubleshooting.
