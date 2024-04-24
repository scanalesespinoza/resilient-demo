
# Resilient Quarkus Demo Setup

This repository provides instructions for setting up a demo environment to showcase resilience features of a Quarkus application with a frontend and backend service using PostgreSQL as the database. The setup uses Podman.

## Prerequisites

- Git
- Podman
- Java (JDK 11 or later)
- Apache Maven

## Setup Instructions

### Clone the Repositories

```bash
git clone https://github.com/scanalesespinoza/resilient-frontend.git
git clone https://github.com/scanalesespinoza/resilient-app.git
```

### Create a New Local Repository

```bash
mkdir resilient-demo
cd resilient-demo
git init
```

### Setup Podman Compose

Create a `podman-compose.yml` with the following content:

```yaml
version: '3.7'
services:
  postgres:
    image: postgres:latest
    environment:
      POSTGRES_DB: resilientdb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    ports:
      - "5432:5432"
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  postgres-data:
```

### Configure Applications

Configure both the frontend and backend applications to connect to the PostgreSQL database.

### Building the Applications

```bash
# Build the frontend
cd resilient-frontend
./mvnw clean package

# Build the backend
cd ../resilient-app
./mvnw clean package
```

### Run the Podman Containers

```bash
cd ../resilient-demo
podman-compose up -d
```

### Run the Applications

```bash
# Run the frontend
cd resilient-frontend
./mvnw quarkus:dev

# Run the backend
cd ../resilient-app
./mvnw quarkus:dev
```

### Monitoring and Observability

Setup Prometheus and Grafana for monitoring the applications.

### Testing Resilience

Simulate various failures and observe the response of the applications.

## Conclusion

This setup demonstrates the resilience features of the applications in handling failures, managing contention, and recovering from incidents effectively.
