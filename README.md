# Setup multi-container project in Docker

1. Mongodb (container - 1)
2. node api (backend container - 2)
3. react app (frontend container - 3)

## Description

Setting up multi-container project using Docker with mongo database, node api and react frontend app.

## Getting Started

### Dependencies

* Docker installed

### Executing program
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



## Help

Any advise for common problems or issues.
```
command to run if program contains helper info
```

## Authors

Contributors names and contact info

ex. Dominique Pizzie  
ex. [@DomPizzie](https://twitter.com/dompizzie)

## Version History

* 0.2
    * Various bug fixes and optimizations
    * See [commit change]() or See [release history]()
* 0.1
    * Initial Release

## License

This project is licensed under the [NAME HERE] License - see the LICENSE.md file for details
