# apache-pulsar-setup
This repository provides a complete Docker-based setup for running **Apache Pulsar** with **Pulsar Manager**, allowing you to manage Pulsar clusters from a user-friendly web dashboard.

---

## 📁 Repository Structure
- `docker-compose.yml` – Docker configuration to spin up Pulsar, Pulsar Manager and supporting services.

---

## 🧰 Requirements

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

---

## ▶️ Getting Started

1. Clone the Repository

   ```bash
   git clone https://github.com/DhanushNehru/apache-pulsar-setup
   cd apache-pulsar-setup
   ```
2. Start the Stack
   ```bash
   docker-compose up -d
   ```
3. Access Pulsar Manager
   URL: http://localhost:9527
   Username: pulsar
   Password: pulsar

---
   
## ⚙️ Post-Startup Setup

- Reload NGINX (if needed)
```bash
docker exec -it dashboard nginx -s reload
```
- Fetch CSRF Token
```bash
curl -X GET http://localhost:7750/pulsar-manager/csrf-token
```

---

## 👤 Create Admin User

- Access container shell:
```bash
docker exec -it dashboard /bin/sh
```
- Run user creation script:
```bash
cd pulsar-manager/pulsar-manager/bin
create-user --name admin --password admin
```
- Restart the container:
```bash
docker restart dashboard
```  
---

## 🔐 Set Superuser

- Option 1: Simple PUT request
```bash
curl -X PUT "http://43.204.103.255:7750/pulsar-manager/users/superuser" \
  -H "Content-Type: application/json" \
  -d '{"name": "admin", "password": "apachepulsar"}'
```

- Option 2: With CSRF Token
```bash
CSRF_TOKEN=$(curl -s http://43.204.103.255:7750/pulsar-manager/csrf-token)
curl -H "X-XSRF-TOKEN: $CSRF_TOKEN" \
     -H "Cookie: XSRF-TOKEN=$CSRF_TOKEN;" \
     -H "Content-Type: application/json" \
     -X PUT http://43.204.103.255:7750/pulsar-manager/users/superuser \
     -d '{"name": "admin", "password": "apachepulsar", "description": "test", "email": "username@test.org"}'
```
---

## 📝 Notes

- External IPs (43.204.*) are for demonstration; replace with your own instance or use localhost.
- Ensure ports 9527 and 7750 are open and not blocked by firewalls.

## 📚 References

- [Apache Pulsar](https://pulsar.apache.org/)
- [Pulsar Manager GitHub](https://github.com/apache/pulsar-manager)
