### Task 1: What is Docker?

- What is a container and why do we need them?
  
  Containers are like light weight,standalone and executable package of software that contains all source code, configuration file and the dependencies for an application to run. It does not have a full-fledge operating system. It shares host machine OS kernel

  We need containers because, they are portable i.e Containers are the best way to build and ship the application. As it contains all the dependencies of the application to run, it solves the problem - " Its works on my computer, but doesnt work on yours". Secondly, Each container runs in an isolated environment ensuring that applications do not interfere with each other and allowing for better security management.


- Containers vs Virtual Machines — what's the real difference?

  Containers are light weight since they share the host OS-kernal to run the application insie it. VMs run on a hypervisor, including a full guest OS.
  In terms of security, virtualization is more secure than containerization. If one of the virtual machines is being exploited, the other virtual machines are still safe because virtual machines are completely separate from each other. With containerization, containers share the same underlying system hardware with the host machine. If one of the containers is exploited, other running containers and the host machine are put at risk.
  Regarding scalability, containerization is much better than virtualization since it does not require a lot of resources to run and does not have much overhead. Also, with the introduction of orchestration tools like Kubernetes, we can quickly scale the containers’ resources and instances to suit our needs.
  
- What is the Docker architecture? (daemon, client, images, containers, registry)

  Docker daemon:- Docker daemon or DockerD is the heart of docker. It is persistent background process that serves as the "brain" of the Docker system.
  It manages the the creation, execution, monitoring, and termination of containers. The Docker daemon listens for Docker API requests which the Docker client sends.

  Docker Client:- The Docker client is the primary way through which Docker users interact with Docker. When we use commands such as docker run, the client sends these commands to dockerd, which carries them out. The docker command uses the Docker API. The Docker client can communicate with more than one daemon.

  Docker Images:- An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization

  Docker containers:- Docker container is a running instance of an image. We can create, start, stop, move, or delete a container using the Docker API or CL. A container is relatively well isolated from other containers and its host machine.

  Docker Registry :- A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker looks for images on DockerHub by default. We can even run your own private registry.


### Task 3: Run Real Containers

1. Run an **Nginx** container and access it in your browser
   - Search and pull the Nginx image from the dockerhub.
   - Run the nginx image with using the command:
     ```
     docker run -d -p 80:80 --name my-nginx-container nginx
     ```
   - Inspect that the container is running by using the command ` docker ps `
   - Access it from browser by hitting the port 80.
     
2. Run an **Ubuntu** container in interactive mode — explore it like a mini Linux machine
   - Pull and run a docker container by using the below command
     ```
     docker run -it -d --name my-ubuntu-container ubuntu:latest
     docker exec -it <container-id> /bin/bash
     ```
     
3. List all running containers
   - To list all the runing containers.
     ```
     docker ps
     ```
     
4. List all containers (including stopped ones)
   - To list all the containers including the stopped ones:
     ```
     docker ps -a
     ```
     
6. Stop and remove a container
   - To stop a running container and remove it:
     ```
     docker stop <container-id>
     docker rm <container-is>
     ```
     
### Task 4: Explore
1. Run a container in **detached mode** — what's different?
   - To run a container in detached mode, we give the flag '-d' to the docker run command
     ```
     docker run -d <image-id>
     ```
     This will run the container in the background.
     
2. Give a container a custom **name**
   - To give a ontainer a custom name, we can use the '--name' flag.
   ```
   docker run -d --name <container-name> <container-id>
   ```
   
3. Map a **port** from the container to your host
   - To map a port from container to port, we use th '-p' flag.
   ```
   docker run -d -p <host_port>:<container_port> <image-id>
   ```

4. Check **logs** of a running container
   - To check the logs of a running container, the command is :
   ```
   docker logs <container-id>
   ```
   
5. Run a command **inside** a running container
   - To run a commnad inside container, we have to exec into the container with interactive command.
     ```
     docker exec -it <container-id> /bin/bash
     ```
   























