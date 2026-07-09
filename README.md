# Eventhub Platform Monorepo

This repository contains the monorepo setup for the Eventhub Platform, including both the Laravel backend and Vue.js frontend.

## Getting Started

### Prerequisites

- Docker
- Docker Compose

### Running the Application

To start the application, run the following command:

```bash
docker-compose up --build
```

This command will build and start the Laravel backend, Vue.js frontend, MySQL, Redis, and MinIO services.

### Accessing the Application

- Laravel backend: `http://localhost`
- Vue.js frontend: `http://localhost:8080`

### Stopping the Application

To stop the application, press `CTRL+C` in the terminal where the application is running or run:

```bash
docker-compose down
```

This will stop and remove the containers.