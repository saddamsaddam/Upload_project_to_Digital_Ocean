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
