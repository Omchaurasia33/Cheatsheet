# Docker Cheat Sheet

## Docker Workflow Summary

### Docker Image Creation
1. Run `Docker Desktop` App.
2. Check Docker status with:
   ```bash
   docker info
   ```
3. Save the `Dockerfile` in the project folder.
4. Build the Docker image:
   ```bash
   docker build -t your-image-name .
   ```

### Save Docker Image to a File
Save the image to a `.tar` file:
```bash
docker save -o myimage.tar myimage:latest
```

### Manage Containers
- **Check Running Containers:**
   ```bash
   docker ps
   ```
- **Check All Containers (Running and Stopped):**
   ```bash
   docker ps -a
   ```
- **Stop a Running Container:**
   ```bash
   docker stop <container_id_or_name>
   ```

### Run Image on a Server
1. Upload the `.tar` file to the server.
2. Load and run the image:
   ```bash
   docker load < myimage.tar
   docker run --name container_name -d -p host_port:container_port --restart unless-stopped image_name:latest
   ```

---

## MMRDA Frontend Update on Server (NGINX)

### Deployment Steps

#### Step 1: Build Docker Image
```bash
docker build -t mmrda_build .
```

#### Step 2: Save Image to `.tar`
```bash
docker save -o mmrda_build.tar mmrda_build:latest
```

#### Step 3: Copy Image to Server
Transfer the `.tar` file to the server using `scp`:
```bash
scp mmrda_build.tar user@172.16.0.40:/path/to/destination
```

#### Step 4: Stop and Remove Old Containers and Images
- Stop the running container:
   ```bash
   docker stop <container_id_or_name>
   ```
- Remove the container:
   ```bash
   docker rm <container_id_or_name>
   ```
- Remove the image:
   ```bash
   docker rmi mmrda_build
   ```
- Clean up unused Docker resources:
   ```bash
   docker system prune -a
   ```
   *Example Output: Total reclaimed space: 18.05GB (Date: 26-12-2024)*

#### Step 5: Load Docker Image
```bash
docker load < mmrda_build.tar
```

#### Step 6: Run Docker Image
Run the Docker image in detached mode:
```bash
docker run -d --name mmrda_build -p 8085:80 mmrda_build
```

---

## Additional Cleanup Commands

- Remove Docker Image:
   ```bash
   docker rmi mmrda_build
   ```
- Stop a Container:
   ```bash
   docker stop <container_id_or_name>
   ```
- Remove a Container:
   ```bash
   docker rm <container_id_or_name>
   ```
- Delete Old Images and Containers:
   ```bash
   docker system prune -a
   ```

