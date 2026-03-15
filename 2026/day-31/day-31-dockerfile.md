## Task 1: Your First Dockerfile
1. Created a folder called `my-first-image`
    
2. Inside it, create a `Dockerfile` that:
   - Uses `ubuntu` as the base image
   - Installs `curl`
   - Sets a default command to print `"Hello from my custom image!"`
   The Dockerfile written,
   ```
   FROM ubuntu AS Builder
    
   WORKDIR /app

   RUN apt-get update /
       && apt install curl -y

   CMD ["echo","Hello from my custom image!"]
   ```

3. Build the image and tag it `my-ubuntu:v1`
   Once the Dockerfile is done, we can build the image using the dockerfile. Dockerfile is a step by step instruction on building the image.
   ```
   docker build -t my-ubuntu:v1 .
   ```
   ![img](img/Picture1.png)

   
4. Run a container from your image
   Run the container, command is:
   ```
   docker run -it -d <image-name>
   ```
   ![img](img/Picture2.png)


---

### Task 2: Dockerfile Instructions
Create a new Dockerfile that uses **all** of these instructions:
- `FROM` — base image
- `RUN` — execute commands during build
- `COPY` — copy files from host to image
- `WORKDIR` — set working directory
- `EXPOSE` — document the port
- `CMD` — default command

```
FROM ubuntu:latest
WORKDIR /app
RUN apt-get update \
    && apt install nginx -y
COPY index.html /var/www/html/index.html
EXPOSE 80
CMD ["nginx","-g","daemon off;"]

```










