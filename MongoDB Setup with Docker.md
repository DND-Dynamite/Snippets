### Installation using docker

- run docker pull command in power-shell

```
docker pull mongo
```

- run mongo-db container

```
docker run -d \
  --name mongodb \
  -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  mongo
```

- verify container

```
docker ps
```

- connecting using shell

```
docker exec -it mongodb mongosh -u admin -p password
```

- connecting using URL

```
mongodb://admin:password@localhost:27017
```

- stop and resume mongodb container

```
docker stop mongodb
docker rm mongodb
```


