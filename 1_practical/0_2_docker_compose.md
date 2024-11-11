# Docker Compose


### What do you need to install for it to work?
Docker Compose, which is included if you have downloaded `Docker Desktop`.

### How to store your docker compose file?
Store `docker-compose.yaml` in the root directory of your project for easy access and version control.

### Find out about these docker compose commands to:


#### manage your application
`docker-compose up`: Builds, (re)creates, and starts containers as defined in docker-compose.yml.
`docker-compose down`: Stops and removes containers, networks, and volumes created by up.

#### start the application (without detached mode)
`docker-compose up`: Runs services and outputs logs to the console.

#### start the application (in detached mode)
`docker-compose up -d`: Runs services in the background.

#### what is the difference between running your application with or without detached mode
**Detached Mode (-d)**: Runs containers in the background, allowing you to use the terminal for other commands.
**Non-Detached Mode**: Runs containers in the foreground, displaying logs and keeping the terminal focused on the running services.

#### stop the application
`docker-compose stop`: Stops running containers but keeps the containers and network in place.

### check services running with docker compose
`docker-compose ps`: Lists all services defined in `docker-compose.yaml` and shows their status.

### view logs in real-time
`docker-compose logs -f`: Shows live logs for all services. Use `-f` (follow) to update logs in real-time. Putting the name of the container after `-f` will let you see just the logs for that container. Useful for if you have a few containers running and need specific information.

### view docker compose images
`docker-compose images`: Lists images used by your Docker Compose services.

# Using Docker Compose to set up the App and DB containers
1. Create a `docker-compose.yaml` file and insert the following:

```dockerfile
version: "3.8" # The version of Docker we want to use
services:
# MongoDB Service
  mongo:
    image: mongo:7.0.6 # Specifies the Mongo Image type
    ports:
      - "27017:27017" # Maps port 27017 to host and container, allowing access to MongoDB
    volumes:
      - mongo-data:/data/db # Mounts the named volume 'mongo-data' to the /data/db directory in the MongoDB container for data persistence.

# Application Service
  app:
    image: <dockerhub-username>/<image-name>
    ports:
      - "3000:3000" # Maps port 3000 on the host and container, allowing access to the app
    environment:
      DB_HOST: "mongodb://mongo:27017/posts" # Environment variable used by the app to connect to MongoDB. The hostname 'mongo' resolves to the MongoDB container within the Docker network. This matches the mongo service above
    depends_on:
      - mongo # Ensures the MongoDB service starts before the app service.
    command: sh -c "node seeds/seed.js && npm start" # Runs the seed script to populate the database, then starts the app with npm

volumes:
  mongo-data:  # Defines the mongo-data volume, allowing data to persist between container restarts
```
*Note! a volume is a storage mechanism used to persist data generated or used by Docker containers.*

2. Use `docker-compose up -d` to run the services, spinning up the containers.
3. Go to the `localhost:3000/posts` webpage and confirm success. 

## Manually seeding the database
1. Input `docker exec -it <container-ID> node seeds/seed.js` into your CLI. This will seed container.

## Automatically seeding the database
1. Insert `command: sh -c "node seeds/seed.js && npm start"` within your app service. `npm start` is required regardless of it being in the image, as the `command:` used in the `docker-compose.yaml` file will override the `CMD` used in the `Dockerfile`.

