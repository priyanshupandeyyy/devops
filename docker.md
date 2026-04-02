# Docker

##### Difference between Dockerfile and docker-compose

“We use Docker Compose instead of just a Dockerfile when the application requires multiple services to run together. A Dockerfile defines how to build one container, while Docker Compose orchestrates multiple containers — such as the backend, database, and frontend — including their networking, environment variables, volumes, and startup order.”

why not use just a big container for all services ->

“Running multiple services in one container breaks separation of concerns. Docker Compose allows each service to be independently built, scaled, restarted, and replaced, which aligns with microservice and cloud-native principles.”

| Tool | Purpose |
| --------------------- | ----------------------- |
| Dockerfile | Build a single image |
| Docker Compose | Run multiple containers |
| Compose ≠ replacement | It’s orchestration |
| Why Compose | Coordination, not build |

**What is Docker**

“Docker is a containerization platform that allows applications and their dependencies to be packaged together into containers, ensuring consistency across development, testing, and production environments.”

**Difference between docker image and container**

A Docker image is an immutable blueprint that defines what goes into a container, while a container is a running instance of that image.

**What is a docker file?**

A Dockerfile defines the steps required to build a Docker image, such as choosing a base image, installing dependencies, copying files, and defining the startup command.

**Why use Docker Compose instead of Dockerfile?**

A Dockerfile is used to build a single container image, while Docker Compose is used to define and run multiple containers together. Compose manages networking, environment variables, volumes, and service dependencies, allowing the entire application stack to be started with one command.

**Can Docker Compose be used in production?**

Yes, Docker Compose can be used in production for small to medium systems, but for large-scale or highly dynamic environments, orchestration tools like Kubernetes are more suitable.

**How do containers communicate in docker compose?**

Docker Compose creates a private network where each service is accessible by its service name as a hostname.

ex : postgresql://user:pass@db:5432/mydb

**How is docker different than a virtual machine?**

Virtual machines include a full operating system, while Docker containers share the host OS kernel, making them more lightweight and faster to start.

**What happens if a container crashes?**

If configured with a restart policy, Docker will automatically restart the container. If the container uses volumes, persistent data remains intact.

**What are docker volumes?**

Docker volumes are used to persist data outside the container lifecycle, allowing data to survive container restarts and recreations.

- **“Docker ensures environment consistency.”**
- **“Compose orchestrates containers; Dockerfile builds them.”**
- **“Containers share the host kernel.”**
- **“Volumes persist data beyond container lifecycle.”**

##### *Common Mistakes*

- Using latest for DB images
- Using localhost inside containers
- Forgetting volumes
- Mixing dev & prod .env
- Assuming Docker DB is production-safe

***Why Db's are different in prod***

“Databases are handled differently in production because they are stateful and critical. In production we usually use managed database services instead of running databases in Docker containers, because managed services provide backups, monitoring, replication, security, and high availability, which are difficult and risky to manage manually.”

##### Namespaces and Cgroups

Namespaces let the kernel gives it's own isolated view of its system, pid, network, mount etc etc..

Cgroups let the kernel limit how much cpu and memory a process can use

Docker caches at all levels so instruction lining matters

as changes in package.json file maynot change hence saving build time dramatically

-p host:container -p will refer to the port while the first port will be the host port and the second will be container

creating a communication bridge between the host and container

when installing docker it creates a default network called a bridge but it only involves ip communication

So we need a custom network so api communication can be handled also providing DNS

Docker compose uses something similar to connect different containers

Docker compose lets us define our entire application stack

every service, network, volume in a single yaml file.

##### Docker Image vs Container

| **Feature** | **Docker Image** | **Docker Container** |
| -------------- | -------------------------------------------- | ------------------------------------------- |
| **Definition** | A packaged, static template. | A running instance of an image. |
| **State** | Immutable (Read-only). | Mutable (Read-write). |
| **Phase** | The Build phase. | The Runtime phase. |
| **Lifespan** | Permanent (until manually deleted). | Ephemeral (temporary by default). |
| **Commands** | docker build, docker pull, docker push | docker run, docker start, docker stop |

##### Docker Registries

A **Registry** is a specialized server that stores and distributes Docker images. A **Repository** is a collection of specific versions (tags) of one image _inside_ that registry.

**1. Public Registries (The Open Library)**

- **Examples:** Docker Hub, GitHub Container Registry (GHCR).
- **Logical Use Case:** Pulling open-source base images (Ubuntu, Node.js, Alpine) or sharing public projects.
- **Critical Flaw:** Zero privacy. Never push proprietary company code or internal secrets here.

**2. Managed Private Registries (The Enterprise Standard)**

- **Examples:** AWS Elastic Container Registry (ECR), Google Artifact Registry (GAR), Azure Container Registry (ACR).
- **Logical Use Case:** Storing proprietary, closed-source applications for professional deployment.
- **Critical Advantage:** The cloud provider handles all hardware scaling, server maintenance, and high availability. You just manage access rules.
- **Characteristics**: secure and controlled access , used in enterprises, stores proprietary applications, Integrated with *IAM and RBAC* -> Identity and Access Management, Role-Based Access Control

**3. Self-Hosted / On-Premises (Maximum Control)**

- **Examples:** Harbor, JFrog Artifactory, Sonatype Nexus.
- **Logical Use Case:** Organizations requiring extreme regulatory compliance, air-gapped networks, or total data sovereignty (e.g., banking, defense).
- **Critical Flaw:** Massive operational overhead. Your team is entirely responsible for server uptime, storage limits, backups, and security patching.

Image naming Convention

images follow a standard naming format:

<registry>/<namespace>/<image>:<tag>

example: docker.io/blank/webapp:v1

not giving label will result in default: latest

##### Docker architecture

- Docker client
- Docker Daemon
- Docker Images
- Container
- Docker Registry

Docker is a platform that uses OS-level virtualization to deliver software in packages called **containers**.

##### 1. Docker Architecture (The Big Picture)

Docker uses a **client-server architecture**. The Docker **Client** talks to the Docker **Daemon**, which does the heavy lifting of building, running, and distributing your Docker containers. They can run on the same system, or you can connect a local client to a remote daemon.

##### 2. Docker Client

The **Client** (docker) is the primary way most users interact with Docker. When you type a command like docker run, the client sends these commands to the **Docker Daemon** (dockerd), which carries them out. The client uses a REST API to communicate with the daemon.

##### 3. Docker Daemon

The **Daemon** is the brain of the operation. It is a background process that manages Docker objects such as images, containers, networks, and volumes. It constantly listens for Docker API requests and handles the complex task of managing the host's operating system resources to keep containers running.

##### 4. Docker Images

An **Image** is a read-only template with instructions for creating a Docker container.

- **The Blueprint:** If a container is a house, the image is the blueprint.
- **Layered File System:** Images are built in layers. For example, an image might start with an Ubuntu layer, add Python on top, and finally add your specific application code.

##### 5. Docker Containers

A **Container** is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI.

- **Isolation:** Containers are isolated from each other and the host machine.
- **Ephemeral:** By default, any data stored in a container disappears when the container is deleted (unless you use "Volumes").

##### 6. Docker Registry

A **Registry** is a storage library for Docker images.

- **Public vs. Private:** **Docker Hub** is the largest public registry that anyone can use. Many companies also use private registries (like AWS ECR or Google Artifact Registry) to keep their proprietary code secure.
- **Push/Pull:** You "pull" an image from the registry to your machine to run it, and "push" your custom images to the registry to share them.

##### Comparison Table: Image vs. Container

| **Feature** | **Docker Image** | **Docker Container** |
| -------------- | ---------------------------- | ------------------------------ |
| **State** | Static (Read-only) | Dynamic (Running) |
| **Analogy** | The Recipe | The Cake |
| **Storage** | Stored in Registry/Disk | Lives in System Memory/Process |
| **Mutability** | Cannot be changed once built | Can be modified while running |

Docker commands :

-it : Runs the container in interactive mode
-d : Runs the container in the background
--name : Specifies a name for the container
--rm : Automatically removes the container when it exists
-p : Maps a host port to a container port
-e / --env : Sets an env variable inside the container
-v : Mounts a host volume inside the container

What are .env variables:

they are key-value pairs used by application to

read configuration

store secrets

control application behavior without changing the image

it enables us to configure environment dynamically

They can be set with **-e*** passed as an argument

docker run -it -e MY_NAME-Priyanshu ubuntu bash

we can view said variables using echo $MY_NAME


MySQL Container Setup

Run the following command to start a MySQL container:

docker run -d \
  -e MYSQL_ROOT_PASSWORD=root124 \
  -e MYSQL_DATABASE=college \
  -e MYSQL_USER=admin \
  -e MYSQL_PASSWORD=admin123 \
  mysql:8
  
Environment Variables Explained
MYSQL_ROOT_PASSWORD → Password for the root user
MYSQL_DATABASE → Database that will be created automatically
MYSQL_USER → Custom user for the database
MYSQL_PASSWORD → Password for the custom user


### Passing Environment Variables from Host System to Docker Container

#### Step 1: Set Environment Variable on Host

**Ubuntu/Linux:**

```bash
export APP_PORT=8000
```

**PowerShell (Windows):**

```powershell
$env:APP_PORT=8000
```

**Check Variable:**

```bash
echo $APP_PORT        # Ubuntu/Linux
echo $env:APP_PORT    # PowerShell
```

---

#### Step 2: Pass Variable to Docker Container

**Ubuntu/Linux:**

```bash
docker run -e APP_PORT=$APP_PORT nginx env
```

**PowerShell (Windows):**

```powershell
docker run -e APP_PORT=$env:APP_PORT nginx env
```

---

### Explanation

* `-e` → Used to pass environment variables to the container
* `APP_PORT` → Variable defined on the host system
* `nginx env` → Runs nginx container and prints environment variables

### Notes

* Make sure the variable is set before running the container
* This method helps in dynamic configuration of containers
* Useful for DevOps and microservices setups

### Class Task 1: Passing Environment Variable in Ubuntu Container

#### Step 1: Run Ubuntu Container with Environment Variable

```bash id="cmd01"
docker run -it -e COLLEGE=CSE ubuntu
```

---

#### Step 2: Verify Inside Container

```bash id="cmd02"
echo $COLLEGE
```

You should see:

```
CSE
```

---

#### Step 3: Stop the Container

```bash id="cmd03"
exit
```

---

### What Happened?

* The environment variable `COLLEGE=CSE` was available **only inside the running container**
* Once the container stops, the variable is **lost**
* If you start a new container, you need to pass the variable again

---

### Key Concept

* Environment variables in Docker are **temporary**
* They exist only for the lifecycle of that container
* This ensures containers remain **stateless and portable**


### Using `.env` File for Environment Variables (Best Practice)

Instead of passing variables manually, you can store them in a `.env` file.

---

#### Step 1: Create `.env` File

```env id="envfile"
APP_PORT=3000
DB_USER=admin
DB_PASS=secret123
```

---

#### Step 2: Run Container Using `.env` File

```bash id="cmd11"
docker run --env-file .env ubuntu
```

**Interactive Mode with Auto Remove:**

```bash id="cmd12"
docker run -it --rm --env-file .env ubuntu
```

---

### Explanation

* `--env-file .env` → Loads all environment variables from the file
* `-it` → Runs container in interactive mode
* `--rm` → Automatically removes container after it stops

---

### Advantages

* Cleaner and more organized configuration
* Easy to manage multiple variables
* Avoids exposing sensitive data in command line
* Reusable across different containers

---

### Notes

* `.env` file should be in the same directory or provide full path
* Keep `.env` file secure (add to `.gitignore` if needed)

