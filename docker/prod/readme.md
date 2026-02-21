# Techibbie Production Stack

This repository contains the **production Docker Compose stack** for Techibbie, including **PostgreSQL**, **MongoDB**, and optional **LocalStack**. The stack is designed to be deployed via **Portainer** and uses **Docker secrets** for secure credentials management.

---

## Contents

- `docker/prod/docker-compose.yml` – Production stack for Portainer
- `.env.default` – Template for environment variables (safe to commit)
- Portainer-managed secrets:
   - `pg_password` – Postgres password
   - `mongo_password` – Mongo root password

---

## Prerequisites

1. Docker installed on the host machine
2. Portainer installed and running
3. Permission to create **Docker secrets** in Portainer

---

## Setup

### 1. Environment Variables

- Copy `.env.default` to `.env` (local reference, optional).
- Update the values for **non-sensitive variables**:

```env
COMPOSE_PROJECT_NAME=techibbie-prod

# PostgreSQL
POSTGRES_USER=your_postgres_user
POSTGRES_DB=your_postgres_db

# MongoDB
MONGO_INITDB_ROOT_USERNAME=your_mongo_user
MONGO_INITDB_DATABASE=your_mongo_db
```

> **Do not put passwords here** — use Portainer secrets instead.

---

### 2. Create Secrets in Portainer

1. Go to **Portainer → Secrets → Add secret**
2. Create the following secrets:

| Secret Name      | Value Description          |
| ---------------- | -------------------------- |
| `pg_password`    | Postgres database password |
| `mongo_password` | MongoDB root password      |

> Secret names **must match** those referenced in `docker-compose.yml`.

---

### 3. Deploy the Stack

1. Go to **Portainer → Stacks → Add stack**
2. Paste the `docker-compose.yml` content from `docker/prod/docker-compose.yml`
3. Set environment variables for **usernames and database names** (from `.env.default`)
4. Deploy the stack

Your containers will automatically read passwords from `/run/secrets/...`.

---

## Service Overview

| Service    | Port  | Description                                                   |
| ---------- | ----- | ------------------------------------------------------------- |
| Postgres   | 5432  | PostgreSQL database, reads password from secret `pg_password` |
| MongoDB    | 27017 | MongoDB database, reads password from secret `mongo_password` |
| LocalStack | 4566  | AWS-compatible local services (optional for dev/testing)      |

---

## Volumes

| Volume      | Purpose                  |
| ----------- | ------------------------ |
| `pgdata`    | Persistent Postgres data |
| `mongodata` | Persistent MongoDB data  |

---

## Notes / Best Practices

- Always use **Portainer secrets** for passwords; never commit them in Git.
- Keep `.env.default` as a **template only**; never include real credentials.
- For updates, redeploy the stack after updating `.env` variables or secrets.
- LocalStack is optional in prod; remove if not needed.

---

## Troubleshooting

- **MongoDB authentication errors:** Check that the `mongo_password` secret matches the username in `.env`.
- **Postgres connection errors:** Ensure `POSTGRES_PASSWORD_FILE` points to `/run/secrets/pg_password`.
- **LocalStack issues:** Only required for dev; ignore in prod.

---

## References

- [Docker Secrets](https://docs.docker.com/engine/swarm/secrets/)
- [Portainer Stacks](https://www.portainer.io/documentation/stacks/)
- [LocalStack](https://docs.localstack.cloud/)
