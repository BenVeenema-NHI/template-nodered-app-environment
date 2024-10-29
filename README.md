# My Docker Project

This project contains a Docker Compose configuration file and documentation for setting up a Node-RED based project in a container. It is setup to create a Node-Red app in production and additionally, a Mosquitto container in development.

## Getting Started

To get started with this project, follow the steps below:

1. Install Docker on your machine if you haven't already.
2. Clone this repository to your local machine.
3. Navigate to the project directory.

## Usage
Once the container is started, you can access the NodeRed environment on `localHost:1880/`. If in development mode, you can access the MQTT broker on `localHost:1883`

### Starting the Node-RED Container
#### Development
To start the environment in development mode run the following command:

```bash
docker-compose up -d --build
```

This command will start the NodeRed and Mosquitto containers in detached mode, allowing it to run in the background.

#### Production
To start the enviroment in production mode run the following command, where XXXX is the desired port to access node-red in production (this assumes it is being run in a linux environment):
##### Linux
```bash
NODE_RED_PORT=XXXX docker-compose -f "docker-compose.yml" up -d --build
```
##### Windows
```bash
$env:NODE_RED_PORT="XXXX"; docker-compose up -d
```
This command will start only the NodeRed container in detached mode.

#### A note on ports
We want to avoid ports for common protocols (0-1023) and stay in the user ports section (1024-49151), however, many of the "user ports" have been attached to common services (node-red: 1880, mqtt: 1883). According [to this stackoverflow](https://stackoverflow.com/questions/10476987/best-tcp-port-number-range-for-internal-applications), the range 29170-29998 has the most open ports in sequence



### Accessing Node-RED

Once the container is running, you can access the Node-RED interface by opening a web browser and navigating to `http://localhost:1880`.

### Stopping the Node-RED Container

To stop the Node-RED container, run the following command:

```bash
docker-compose down
```

This command will stop and remove the container.

## Configuration

The Docker Compose configuration file `docker-compose.yml` defines the services, networks, and volumes for the Docker project. Here is an overview of the configuration:

```yaml
version: '3'
services:
  node-red:
    image: nodered/node-red
    ports:
      - 1880:1880
    volumes:
      - ./data:/data
```

The `node-red` service uses the `nodered/node-red` image and exposes port `1880`. It also creates a volume named `data` that maps to the `./data` directory in the project.

## License

This project is currently unlicensed.