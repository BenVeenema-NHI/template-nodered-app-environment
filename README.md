# My Docker Project

This project contains a Docker Compose configuration for setting up a Node-RED based project in a container, backed by a PostgreSQL database managed by Liquibase. In production, the stack runs Node-RED, PostgreSQL, and the Liquibase db-manager. In development, a Mosquitto MQTT broker is additionally started via the override file.

## Getting Started

To get started with this project, follow the steps below:

1. Install Docker on your machine if you haven't already.
2. Clone this repository to your local machine.
3. Navigate to the project directory.

## Usage
Once the container is started, you can access the Node-RED environment on `localhost:1880/`. If in development mode, you can access the MQTT broker on `localhost:1883`.

### Starting the Node-RED Container
#### Development
To start the environment in development mode run the following command:

```bash
docker compose up -d --build
```

This command will start the Node-RED, PostgreSQL, db-manager, and Mosquitto containers in detached mode, allowing them to run in the background.

#### Production
To start the environment in production mode run the following command, where XXXX is the desired port to access Node-RED (this assumes it is being run in a Linux environment):
##### Linux
```bash
NODE_RED_PORT=XXXX docker compose up -d --build
```
##### Windows
```bash
$env:NODE_RED_PORT="XXXX"; docker compose up -d --build
```
This command will start the Node-RED, PostgreSQL, and db-manager containers in detached mode.

#### A note on ports
We want to avoid ports for common protocols (0-1023) and stay in the user ports section (1024-49151), however, many of the "user ports" have been attached to common services (node-red: 1880, mqtt: 1883). According [to this stackoverflow](https://stackoverflow.com/questions/10476987/best-tcp-port-number-range-for-internal-applications), the range 29170-29998 has the most open ports in sequence.



### Accessing Node-RED

Once the container is running, you can access the Node-RED interface by opening a web browser and navigating to `http://localhost:1880`.

### Stopping the Node-RED Container

To stop the Node-RED container, run the following command:

```bash
docker compose down
```

This command will stop and remove the containers.

## Configuration

The Docker Compose configuration file `docker-compose.yml` defines the services, networks, and volumes for the Docker project. Here is an overview of the configuration:

```yaml
name: my-project

services:
  db:
    build: ./db
    env_file:
      - .env
    volumes:
      - ./db_data:/var/lib/postgresql/data

  db-manager:
    build: ./db-manager
    env_file:
      - .env
    depends_on:
      db:
        condition: service_healthy

  node-red:
    build: ./node-red
    ports:
      - "${NODE_RED_PORT:-1880}:1880"
    volumes:
      - node-red-data:/data
      - ./node-red/settings.js:/data/settings.js

volumes:
  node-red-data:
```

The `node-red` service builds from the local `./node-red` directory and exposes port `1880` (configurable via the `NODE_RED_PORT` environment variable). Node-RED data is persisted in a named Docker volume. The `settings.js` file is bind-mounted so it can be version-controlled separately from the flow data.

The `db` service runs PostgreSQL and persists data to `./db_data`. The `db-manager` service runs Liquibase to apply schema migrations on startup.

## License

This project is currently unlicensed.
