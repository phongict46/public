**Refer:**
[Link](https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes)

##### Remove docker container:
```bash
docker ps -a "show list"
docker rm ID_or_Name ID_or_Name "remove images"
```

##### Remove docker images:
```bash
docker images -a "show list"
docker rmi Image Image "remove images"
```