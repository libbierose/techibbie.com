# Techibbie Dev Stack

This repository contains the **development Docker Compose stack** for Techibbie, including **PostgreSQL**, **MongoDB**, and **LocalStack** for AWS service simulation. This stack is intended for **local development**.

---

## Contents

- `docker/dev/docker-compose.yml` – Development stack for local Docker/Portainer
- `.env.default` – Template for environment variables (safe to commit)

---

## Prerequisites

1. Docker installed on your development machine
2. Optional: Portainer for stack management
3. Python installed (if using LocalStack CLI locally)

---

## Setup

### 1. Environment Variables

- Copy `.env.default` to `.env`
- Update the values as needed:

```env
COMPOSE_PROJECT_NAME=techibbie-dev

# PostgreSQL
POSTGRES_USER=dev_user
POSTGRES_PASSWORD=dev_password
POSTGRES_DB=dev_db

# MongoDB
MONGO_INITDB_ROOT_USERNAME=dev_user
MONGO_INITDB_ROOT_PASSWORD=dev_password
MONGO_INITDB_DATABASE=dev_db

# LocalStack (optional)
AWS_ACCESS_KEY_ID=test
AWS_SECRET_ACCESS_KEY=test
AWS_DEFAULT_REGION=eu-west-2
LOCALSTACK_SERVICES=s3
```

> This file is safe to commit since it uses **fake credentials** for dev.

---

### 2. Start the Stack

**With Docker Compose CLI:**

```bash
docker compose --env-file .env up -d
```

**With Portainer:**

1. Go to **Portainer → Stacks → Add stack**
2. Paste the `docker-compose.yml` from `docker/dev/docker-compose.yml`
3. Set environment variables from `.env`
4. Deploy the stack

---

## Service Overview

| Service    | Port  | Description                      |
| ---------- | ----- | -------------------------------- |
| Postgres   | 5432  | Local PostgreSQL database        |
| MongoDB    | 27017 | Local MongoDB database           |
| LocalStack | 4566  | AWS service simulator (S3, etc.) |

---

## Volumes

| Volume            | Purpose                     |
| ----------------- | --------------------------- |
| `pgdata`          | Persistent Postgres data    |
| `mongodata`       | Persistent MongoDB data     |
| `localstack-data` | LocalStack persistent state |

---

## Notes / Best Practices

- LocalStack is optional; only needed for testing AWS services locally.
- You can safely destroy and recreate containers; volumes persist database data.
- Use `.env` to configure usernames, passwords, and LocalStack services.
- This stack is **for development only** — do not use dev credentials in production.

---

## Troubleshooting

- **MongoDB authentication errors:** Check `MONGO_INITDB_ROOT_PASSWORD` in `.env`.
- **Postgres connection errors:** Check `POSTGRES_PASSWORD` in `.env`.
- **LocalStack not responding:** Ensure Docker ports 4566 (and optional range 4510–4559) are available.

---

## References

- [Docker Compose](https://docs.docker.com/compose/)
- [LocalStack](https://docs.localstack.cloud/)
- [Portainer Stacks](https://www.portainer.io/documentation/stacks/)
