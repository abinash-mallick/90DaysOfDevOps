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

### Task 3: CMD vs ENTRYPOINT
1. Create an image with `CMD ["echo", "hello"]` — run it, then run it with a custom command. What happens?
   The predefined commands inside CMD gets overridden by the custom command and it is executed.
   For Ex:

    FROM ubuntu-latest
    CMD ["echo","Hello WOrld"]
    
     If we run: `docker run myimage echo bye`
    
     Container Output will be: `bye`
    
    The arguments inside CMD got overridden.
   
3. Create an image with `ENTRYPOINT ["echo"]` — run it, then run it with additional arguments. What happens?
   The additional arguments are appended to the command defined inside ENTRYPOINT and it is executed as a whole.
   For ex:

    FROM ubuntu-latest
    ENTRYPOINT ["echo"]
    
   If we run: `docker run myimage hello`
    
   Container Output will be: `hello`
    
   The command `echo` caannot be replaced. Only the arguments changes.

   
4. Write in your notes: When would you use CMD vs ENTRYPOINT?

ENTRYPOINT is more like hardcoding the command. It cannot be overridden by arguments passed during container execution.

On the other hand, the commands inside CMD can be overridden.


- We use CMD when we want flexible defaults that users can override easily.
- We use ENTRYPOINT when we want the container to behave like a specific executable.
- We can combine both for maximum control: ENTRYPOINT defines the command, CMD provides default arguments.


### Task 4: Build a Simple Web App Image
1. Create a small static HTML file (`index.html`) with any content
   vi index.html
   
2. Write a Dockerfile that:
   - Uses `nginx:alpine` as base
   - Copies your `index.html` to the Nginx web directory
   ```
   FROM nginx:alpine AS BaseImage
   WORKDIR /app
   COPY index.html /usr/share/nginx/html/index.html
   RUN apt-get update
   EXPOSE 80
   CMD ["nginx","-g","daemon off"]
   ```
   
3. Build and tag it `my-website:v1`
   To build the image we can use the below command:
   ```
   docker build -t my-website:v1 .
   ```
   
4. Run it with port mapping and access it in your browser
   To run the image with port mapping :
   ```
   docker run -it -d -p 80:80 my-website:v1
   ```
   































