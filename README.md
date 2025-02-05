................................Summary............................................


.........................    Working process:......................................

1.   running container delete
   
2.   running image delete
  
3.   source code uploaed

4.   build image

5.   run container

6.   domain check



...................................Commands.........................................



Step1: loging

Step2:Move to project directory :

   root@ubuntu-cit-ocean:cd /www/wwwroot/snvn.deepseahost.com/DeviceManagement/
   
Step3: check running and stopped container:

   docker ps -a
   
step4: check images:

   docker images
   
step5: create image by building with name(tag) and version:

   docker build -t snvn:v1    //  docker build -t snvn

step6: check container and images again

step7: run image with verion:

   docker run --name snvnv1 -d -p 8080:8080 snvn:v1  //  docker run --name snvnv1 -d -p 8080:8080 snvn
   
step8: chek container to confirm run or not:

   docker ps -a
   
step9: check website by deleting cashe.


some important command:

Remove the container:docker rm -f <container_id_or_name>

Remove image: docker rmi <id or name>

Start container: docker start <id or name>

Stop container: docker restart <id or name>


Thanks

saddamnvn



...................................///................................................





which will delete first container or image?


When using `docker rmi` (to remove an image), Docker will first check if there are any containers that are using that image. If there are stopped or running containers associated with the image, Docker will prevent the image from being deleted.

Therefore, **you must delete the containers first** before you can remove the image. Here's the correct sequence:

1. **Stop and remove containers**:
   ```bash
   docker stop <container_id>
   docker rm <container_id>
   ```

2. **Remove the image**:
   ```bash
   docker rmi <image_id>
   ```

If you attempt to remove an image while a container is still using it, Docker will show an error like:

```
Error response from daemon: conflict: unable to delete <image_id> (cannot be forced) - image is being used by stopped container <container_id>
```

So, to avoid this error, ensure you remove the containers first, then the image.

Let me know if you'd like further clarification!

The command `docker ps -a` lists **all** Docker containers on your server, including both **running** and **stopped** containers.  

### **Run the command:**  
```sh
docker ps -a
```
### **Output Explanation:**
You'll see a table like this:  

| CONTAINER ID | IMAGE         | COMMAND                  | CREATED        | STATUS                     | PORTS           | NAMES      |
|-------------|--------------|-------------------------|---------------|---------------------------|---------------|------------|
| 123abc456def | my-app-image | "java -jar app.jar"     | 2 hours ago   | **Up 2 hours**            | 8080->8080/tcp | my-app     |
| 789xyz123ghi | nginx        | "nginx -g 'daemon off;'" | 5 days ago    | **Exited (0) 3 days ago**  | 80->80/tcp     | web-server |

### **Key Columns:**
- **CONTAINER ID** → Unique ID of the container  
- **IMAGE** → The image used to start the container  
- **COMMAND** → The command that was run inside the container  
- **CREATED** → How long ago the container was created  
- **STATUS** → Shows whether the container is **running**, **stopped**, or **exited**  
- **PORTS** → Lists the mapped ports (e.g., `8080->8080/tcp` means app is accessible at port `8080`)  
- **NAMES** → The assigned name of the container  

### **If Your App is Running in Docker**
If your Spring Boot app is inside a Docker container, check its status.  
To restart it, use:
```sh
docker restart <CONTAINER_ID>
```
To stop it, use:
```sh
docker stop <CONTAINER_ID>
```
To remove it, use:
```sh
docker rm <CONTAINER_ID>
```

Let me know what you're trying to do next! 🚀

The `stop` and `remove` commands in Docker have different purposes for managing containers. Here's a breakdown of what each one means:

### **1. `docker stop`**
- **Purpose:** Stops a running container without deleting it.  
- **What it does:** When you stop a container, Docker sends a signal to the container to shut it down. The container stops executing its process, but all of its files and configurations remain intact. You can restart the container later using `docker start`.
- **Use case:** If you want to temporarily stop the container without losing any of its data or configuration, you would use `docker stop`.

#### Example:
```sh
docker stop <container_id_or_name>
```
- This will stop the container, but the container will still exist on your system, and you can restart it later.

---

### **2. `docker rm`**
- **Purpose:** Deletes a stopped container from your system.  
- **What it does:** Removes the container and all of its associated data (such as logs, file system, etc.). This operation **permanently deletes** the container, and it cannot be restarted after being removed unless you recreate it.  
- **Use case:** If you no longer need a container or if you want to clear up disk space, you would use `docker rm`. This is often done after stopping a container.

#### Example:
```sh
docker rm <container_id_or_name>
```
- This will remove the stopped container from your system completely. You cannot start it again after it's been removed unless you create a new container from the same image.

---

### **Combined Use (`docker stop` + `docker rm`)**
Sometimes, you'll want to stop and then completely remove a container. You can do this in two steps:
1. Stop the container:
   ```sh
   docker stop <container_id_or_name>
   ```
2. Remove the container:
   ```sh
   docker rm <container_id_or_name>
   ```

Alternatively, you can do both in one step with `docker rm`'s `-f` (force) option:
```sh
docker rm -f <container_id_or_name>
```
- This will stop and remove the container in one command.

---

### Summary:
- **`docker stop`**: Stops the container but keeps it on the system for later use.
- **`docker rm`**: Removes the container permanently from the system, along with its data.

If you want to restart a container after stopping it, you need to use `docker start`.

Yes, you can remove multiple Docker images with a single command.

To remove more than one image, you can provide the image IDs or names separated by spaces. Here's how you can do it:

### **Remove multiple images at once:**
```sh
docker rmi <image_id_or_name1> <image_id_or_name2> <image_id_or_name3> ...
```

### **Example:**
If you want to remove three images:
```sh
docker rmi image1 image2 image3
```

This will remove all three images at once.

---

### **Remove all unused images:**
If you want to remove **all unused Docker images** (dangling images), you can use the `-f` flag with the `docker image prune` command:
```sh
docker image prune -a
```
- This removes all images that are **not being used by any container**, freeing up disk space.

---

### **Remove images by filters:**
You can also remove images by certain filters like before a specific date or based on a tag:
```sh
docker rmi $(docker images -f "dangling=true" -q)
```
This command removes **all dangling (unused)** images.

Make sure to verify the images you are about to delete, as this action is irreversible. 

You can get all container IDs in one line by using this command:

```sh
docker ps -a --format "{{.ID}}" | tr '\n' ' '
```

This will print all container IDs on a single line, separated by spaces:

```
0b7d92aa3f6b e1d9ca231162 8665973426c8 c67564d4b368 7bdcbf19cc16 a4be6694a9a9 eb250f743f06 d20f2c40a3ad b30d7d982fe9 b1f336136209 811f84ab762a 03a324a44153 58e2f70e3581 2142e39a2c2b 8600505400c9 9c79eb132598 97876279385d 359da58cb682 7f7e56756521 cbeef9c3fb85 872fd2a77f84 366c033f410c adc80422bc9a 70be081dbbc4 24ac0c898007 0c57153238d1 a95e2dc0a721 44a356ab2489 f8b9058787fa 8a9c69ae9a1c
```

The `docker run -p 8080:8080 snvn` command is used to run a Docker container from the image `snvn` and map port `8080` on your local machine to port `8080` inside the container.

### Breakdown of the command:
- `docker run`: Tells Docker to run a container.
- `-p 8080:8080`: This flag binds port `8080` on your local machine (host) to port `8080` inside the container. This is useful for exposing services (like a web server) running inside the container to your local machine.
  - The format is `-p <host-port>:<container-port>`.
  - In this case, the container's service (perhaps a Spring Boot app, for example) is running on port `8080`, and you can access it on `localhost:8080` or `<your-server-ip>:8080`.
- `snvn`: This is the name of the image from which the container is created. Replace `snvn` with your actual image name if needed.

### Example usage:
1. Make sure you have built the Docker image (`snvn` in this case).
2. Run the command:
   ```bash
   docker run -p 8080:8080 snvn
   ```
3. You can now access the application running inside the container by visiting `http://localhost:8080` (on your local machine) or `http://<your-server-ip>:8080` (on your server).

### Additional options:
- If you want the container to run in the background, you can add the `-d` (detached mode) flag:
  ```bash
  docker run -d -p 8080:8080 snvn
  ```
 docker run -p 8080:8080 snvn    vs  docker run -d -p 8080:8080 snvn 
 The two commands `docker run -p 8080:8080 snvn` and `docker run -d -p 8080:8080 snvn` are similar, but there is a key difference between them:

### 1. `docker run -p 8080:8080 snvn`
- This command will run the Docker container **in the foreground** (attached mode).
- The command will output logs and other information directly to your terminal.
- You will need to manually stop the container by pressing `Ctrl+C` or using another command (like `docker stop`).

### 2. `docker run -d -p 8080:8080 snvn`
- This command will run the Docker container **in the background** (detached mode), due to the `-d` (detached) flag.
- It will not output logs directly to the terminal. The container will run in the background, and you can continue using your terminal for other tasks.
- You can view logs by using `docker logs <container-id>` and manage the container separately (e.g., start, stop, etc.).

### Key Difference:
- **Foreground (default)**: You see logs directly in the terminal, and the container runs attached to your session.
- **Detached**: The container runs in the background, freeing up the terminal for other tasks.

### Example:
- **Foreground**:
  ```bash
  docker run -p 8080:8080 snvn
  ```
  - You'll see logs in the terminal and need to exit the container manually.
  
- **Detached**:
  ```bash
  docker run -d -p 8080:8080 snvn
  ```
  - The container runs in the background, and the terminal is available for other commands. You can check the logs later.

If you plan to run the container for a long time or need to keep using the terminal, the **detached** mode (`-d`) is generally preferred.

if i remove container then images will remove?
No, **removing a container** will not automatically remove the associated **image**. 

Here’s the difference:
- **Container**: A container is a running or stopped instance of an image. You can remove a container, but the image will remain available for creating new containers.
- **Image**: The image is the blueprint from which containers are created. Even if you remove a container, the image remains on your system until you explicitly remove it.

### Example:
1. **Remove container**:
   ```bash
   docker rm <container-id>
   ```
   This removes the container but does not affect the image.

2. **Remove image**:
   ```bash
   docker rmi <image-id>
   ```
   This removes the image. If there are containers still using the image, you'll need to stop or remove those containers before you can remove the image.

So, **removing a container does not delete the image**. If you want to remove both, you'll need to remove the container first, then the image separately.

will one more container for a specific image?
Yes, you can run **multiple containers** from the **same image**. Docker allows you to create and run as many containers as you need from a single image.

For example, if you have an image called `snvn`, you can run multiple containers using the same image, each with a different name or different port mapping.

### Example:
1. **Run the first container**:
   ```bash
   docker run -d -p 8080:8080 --name container1 snvn
   ```
   This will run a container in the background with port 8080 mapped to the host.

2. **Run a second container** using the same image**:
   ```bash
   docker run -d -p 8081:8080 --name container2 snvn
   ```
   This will run another container from the same `snvn` image but map port 8081 on the host to port 8080 inside the container.

As you can see, you can create as many containers as you like from the same image. However, if you are binding container ports to host ports, make sure the host ports are **unique** (since you cannot bind the same host port to multiple containers).

