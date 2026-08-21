# Docker Reverse Proxy

## Objective

Deploy Nginx as a reverse proxy in front of a Docker application container.

The goal of this ticket is to understand the basic request flow:

Client → Nginx → Application

---

## Architecture

```text
Client
   |
   | HTTP :8081
   v
reverse-proxy
   |
   | Docker network: proxy-lab
   v
app
   |
   | Port 80
   v
whoami
