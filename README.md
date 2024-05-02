
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

### Configure Applications

Configure both the frontend and backend applications to connect to the PostgreSQL database.

### Building the Applications

```bash
# Build the frontend
cd resilient-frontend
./mvnw clean package

# Build the backend
cd ../resilient-app
./mvnw clean package -DskipTests 
```

### Run the Podman Containers

In linux terminal

```bash
cd ../../resilient-demo
# Create a volume for PostgreSQL data
podman volume create postgres-data

# Run PostgreSQL and mount the init.sql file from the corrected path
podman run -d --name postgres \
    -e POSTGRES_DB=resilientdb \
    -e POSTGRES_USER=user \
    -e POSTGRES_PASSWORD=password \
    -p 5432:5432 \
    -v postgres-data:/var/lib/postgresql/data \
    -v $(pwd)/../resilient-app/src/main/resources/init.sql:/docker-entrypoint-initdb.d/init.sql \
    postgres:latest
```

In powershell terminal

```bash
cd ../../resilient-demo
# Create a volume for PostgreSQL data
podman volume create postgres-data

$CurrentPath = (Get-Location).Path
$InitSqlPath = Join-Path -Path $CurrentPath -ChildPath "..\resilient-app\src\main\resources\init.sql"
podman run -d --name postgres `
    -e POSTGRES_DB=resilientdb `
    -e POSTGRES_USER=user `
    -e POSTGRES_PASSWORD=password `
    -p 5432:5432 `
    -v "postgres-data:/var/lib/postgresql/data" `
    -v "${InitSqlPath}:/docker-entrypoint-initdb.d/init.sql" `
    postgres:latest
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
