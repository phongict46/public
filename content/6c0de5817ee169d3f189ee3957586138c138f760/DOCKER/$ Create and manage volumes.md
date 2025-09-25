## Create and manage volumes | Step By Step Instructions

To know more about Docker commands refer to the [Docker – Instruction Commands.](https://www.geeksforgeeks.org/docker-instruction-commands)

### ****Step 1: Display all the existing Docker Volumes****

To display all the existing [Docker Volumes,](https://www.geeksforgeeks.org/what-is-docker-volume/) you can use the list command as follows.

```bash
sudo docker volume ls 
```

![Volume List](https://media.geeksforgeeks.org/wp-content/uploads/20201024124424/Screenshotfrom20201024124407-660x238.png)

### ****Step 2: Creating a Volume****

To create a new Docker Volume, you can use the Volume Create Command.

```bash
sudo docker volume create geeksforgeeks
```
### ****Step 3: Inspecting Docker Volumes****

To get the details of the Volumes you have created, you can use the Volume Inspect Command.

```bash
sudo docker volume inspect geeksforgeeks 
```

![Volume Inspect](https://media.geeksforgeeks.org/wp-content/uploads/20201024124911/Screenshotfrom20201024124846-660x269.png)

### ****Step 4: Mounting Docker Volumes****

After creating the Volume, the next step is to mount the Volume to Docker Containers. We will create a Docker Container with the Ubuntu base Image which you will mention in [Dockerfile](https://www.geeksforgeeks.org/docker-concept-of-dockerfile) and mount the _****geeksforgeeks****_ Volume to that Container using the -v flag. You can [install the Linux package](https://www.geeksforgeeks.org/how-to-install-linux-packages-inside-a-docker-container) by using the Dockerfile.
```bash
sudo docker run -it -v geeksforgeeks:/shared-volume --name my-container-01 ubuntu  
```

The above command mounts the _****geeksforgeeks****_ volume to a directory called ****shared-volume**** inside the Docker Container named ****my-container-01.****

![Mounting Docker Volumes](https://media.geeksforgeeks.org/wp-content/uploads/20201024125400/Screenshotfrom20201024125343-660x92.png)

### ****Step 5: Create a file inside the Docker Volume****

 Inside the bash of the Container, create a new file and add some content.

 
```bash
ls  
cd /shared-volume  
echo “GeeksforGeeks” > geeksforgeeks.txt  
ls  
exit 
```

![Creating a file](https://media.geeksforgeeks.org/wp-content/uploads/20201024125653/Screenshotfrom20201024125626-660x164.png)

### ****Step 6: Create another Container and Mount the Volume****

Create another Docker Container called ****my-container-02 geeks for geeks**** inside the Container.

 
```bash
sudo docker run -it -v geeksforgeeks:/shared-volume --name my-container-02 ubuntu 
```

![Verifying Shared Contents](https://media.geeksforgeeks.org/wp-content/uploads/20201024130125/Screenshotfrom20201024130105-660x96.png)

If you go to the ****shared-volume**** directory and list the files, you will find the ****geeksforgeeks.txt**** file that you had created in the same volume but mounted in ****my-container-01**** earlier and it also has the same content inside it. This is so because the volume is shared among the two Containers.