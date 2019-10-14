Title: Docker Compose Mongodb Mongoexpress
Date: 2019-03-14T11:43:22+08:00
Category: docker
Tags: docker, docer-compose, mongoDB, mongoexpress
Author: Jack

The following docker compose file, starts 2 docker containers MongoDB and Mongo-Express (a web-based MongoDB Management system).

docker-compose.yml
```yaml
version: "3"

services:
  mongodb:
    image: mongo
    container_name: mongodb
    ports:
      - "27017:27017"

  mongoex:
    image: mongo-express
    container_name: mongo-express
    ports:
      - "8081:8081"
    links:
      - 'mongodb:mongo'
    depends_on:
      - mongodb
    environment:
      - 'ME_CONFIG_BASICAUTH_USERNAME=admin'
      - 'ME_CONFIG_BASICAUTH_PASSWORD=admin'
```

You can run it with
```bash
docker-compose up
```

Open browser at localhost:8081, Mongo-Express Login page should be loaded in a second. Then you may login with username and passwrod (admin/admin for example)
