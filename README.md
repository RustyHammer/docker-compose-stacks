# Docker Compose Stacks

A collection of ready-to-use Docker Compose stacks for local development.

Each stack is self-contained, documented and follows best practices: no hardcoded secrets, healthchecks, pinned image versions.

> Project built as a hands-on exercise during the [DevOps Bootcamp by TechWorld with Nana](https://www.techworld-with-nana.com/) (2025).
> Implementation, documentation and improvements are my own work.

---

## Available Stacks

| Stack | Services | Ports |
|-------|----------|-------|
| [node-mongo](node-mongo/) | Node.js + MongoDB + Mongo Express | `3000`, `27017`, `8081` |
| [java-postgres](java-postgres/) | Java Spring Boot + PostgreSQL | `8080`, `5432` |

## Quick Start

```bash
# 1. Choose a stack
cd node-mongo/

# 2. Copy the environment file and fill in the values
cp .env.example .env

# 3. Start the stack
docker-compose up -d

# 4. Check that everything is running
docker-compose ps
```

## Best Practices Applied

### Externalized Secrets
Passwords are never hardcoded in Compose files. We use a `.env` file (added to `.gitignore`) with a `.env.example` as a template.

### Healthchecks
Each service has a Docker healthcheck to ensure it is actually operational, not just "started". Dependent services use `depends_on` with `condition: service_healthy`.

### Pinned Image Versions
We use `mongo:7.0` instead of `mongo` (tag `latest`). This avoids surprises when a new major version breaks compatibility.

### Named Networks
Each stack has its own named network to isolate services and prevent conflicts between stacks.

## Structure

```
.
├── node-mongo/
│   ├── docker-compose.yaml
│   ├── .env.example
│   └── README.md
├── java-postgres/
│   ├── docker-compose.yaml
│   ├── .env.example
│   └── README.md
├── .github/
│   └── workflows/
│       └── validate.yml       # CI: Docker Compose validation
├── .gitignore
└── README.md
```
