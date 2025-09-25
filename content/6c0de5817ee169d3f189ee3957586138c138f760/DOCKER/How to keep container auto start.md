Refer: [link](https://stackoverflow.com/questions/30449313/how-do-i-make-a-docker-container-start-automatically-on-system-boot)

Case 1:  
docker has [restart policies](https://docs.docker.com/engine/reference/run/#restart-policies-restart) such as `docker run --restart=always` that will handle this. This is also available in the [compose.yml config file](https://docs.docker.com/compose/compose-file/#/deploy) as `restart: always`. 

Case 2: 
If container already created can use command on server run Docker
```bash
docker ps "show ID_Container, condition container must running"

docker update --restart=always Container_ID
```