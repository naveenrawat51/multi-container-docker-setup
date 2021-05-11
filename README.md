# Setup multi-container project in Docker

1. Mongodb (container - 1)
2. node api (backend container - 2)
3. react app (frontend container - 3)

## Description

Setting up multi-container project using Docker with mongo database, node api and react frontend app.

## Getting Started

### Dependencies

- Docker installed

### Executing program with specifying the port

branch name: 'add-dockerfile'

change backend/app.js file line no. 87 to 'mongodb://host.docker.internal:27017/course-goals'. replace 'localhost' to 'host.docker.internal' to access the docker to host port database mongodb.

```
Docker pull mongo  (to pull the docker image from docker hub)
Docker run --name mongodb --rm -d -p 27017:27017 mongo (run mongo image docker container with mongodb name and 27017 port, --rm to remove container once stopped, -d to run in deattached mode)
```

```
Docker build -t goals-node  (build node api image with goals-node name)
Docker run --name goals-backend --rm -d -p 80:80 goals-node (run goals-node image docker container with goals-backend name and 80 port, --rm to remove container once stopped, -d to run in deattached mode)
```

```
Docker build -t goals-react  (build react app image with goals-react name)
Docker run --name goals-frontend --rm -d -it -p 3000:3000 goals-react (run goals-react image docker container with goals-frontend name and 3000 port, --rm to remove container once stopped, -d to run in deattached mode, run with -it mode otherwise it might not work)
```

We are providing the post while running the containers so that we can open the same port on our host machine browser. 'host.docker.internal' is to communication between docker container and host.

### Executing program with docker network

branch name: 'add-docker-network'

change backend/app.js file line no. 87 to 'mongodb://mongodb:27017/course-goals'. replace 'localhost' to 'mongodb' to access the mongodb container internally for node api.

create a docker network so that containers can communicate with each other without specifying the port

```
Docker network create goals-net (create docker network goals-net)
```

```
Docker pull mongo  (don't run if you have image already ---- to pull the docker image from docker hub)
docker run --name mongodb --rm -d --network goals-net mongo (run mongo image docker container with mongodb name and goals-net network, --rm to remove container once stopped, -d to run in deattached mode)
```

```
Docker build -t goals-node  (need tp run as we changed app.js file --- build node api image with goals-node name)
docker run --name goals-backend -d --rm --network goals-net -p 80:80  goals-node (run goals-node image docker container with goals-backend name and 80 port(will be accessed by react app) and goals-network(will be used to access mongo in container), --rm to remove container once stopped, -d to run in deattached mode)
```

```
Docker build -t goals-react  (build react app image with goals-react name)
docker run --name goals-frontend -d --rm -p 3000:3000 -it goals-react (run goals-react image docker container with goals-frontend name and 3000 port, --rm to remove container once stopped, -d to run in deattached mode, run with -it mode otherwise it might not work)
```

using docker network to access containers internally. we need to specify the port for api as react app runs on host it needs to access the api on host. we need to run react-app with port 3000 so that it'll br running on host

### add volumes to persistent the data

```
docker run --name mongodb -v data:/data/db -d --rm --network goals-net  mongo
```

```
docker run --name mongodb -v data:/data/db -d --rm --network goals-net -e MONGO_INITDB_ROOT_USERNAME=naveen -e MONGO_INITDB_ROOT_PASSWORD=abcd mongo  (you can add username and password as environment variable and change the mongodb connection string accordingly: mongodb://<username>:<password>@mongodb:27017/course-goals)
```

### add volumes bind mount for hot reloading

1. install nodemon for node api(volumes with full path are bind mounts)
2. add CHOKIDAR_USEPOLLING=true as environment variable

```
docker run --name goals-backend -v logs:/app/logs -v C:\Users\narawat\Downloads\multi-01-starting-setup\multi-01-starting-setup\backend:/app -v
/app/node_modules -d --rm -p 80:80 --network goals-net goals-node
```

```
docker run --name goals-frontend -d --rm -v C:\Users\narawat\Downloads\multi-01-starting-setup\multi-01-starting-setup\frontend\src:/app/src -p 3000:3000 -it -e CHOKIDAR_USEPOLLING=true goals-react
```

Note: Running docker with 'MONGO_INITDB_ROOT_USERNAME and MONGO_INITDB_ROOT_PASSWORD' environment variable will make mongodb running with --auth mode.
