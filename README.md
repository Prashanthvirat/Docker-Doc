# Docker-Doc

Task 4 — Docker Deep Dive Documentation
1) Explain the Problem Docker Solves

Before Docker, developers faced these problems:

❌ Common Issues (Without Docker)

Works on my machine but not on server

Dependency conflicts (Node 14 vs Node 18, Python 3.8 vs 3.11)

Hard to set up same environment for all developers

Deployments are slow and inconsistent

Installing apps requires many manual steps

✅ What Docker Solves

Docker solves these problems by packaging the application with:

Code

Dependencies

Runtime (Node, Java, Python, etc.)

System libraries

So the app runs same everywhere:

Laptop

VM

Cloud

Production server

2) Virtual Machines vs Docker
🖥 Virtual Machines (VM)

A VM runs a full OS inside another OS.

Example:

Windows laptop → VirtualBox → Ubuntu VM → App runs inside Ubuntu

✅ Pros:

Strong isolation

Can run different OS kernels

❌ Cons:

Heavy (GBs)

Slow startup (minutes)

Uses more RAM/CPU

🐳 Docker Containers

Docker containers share the host OS kernel and only package what is needed.

Example:

Ubuntu host → Docker → Container → App runs

✅ Pros:

Lightweight (MBs)

Starts in seconds

Easy to move between systems

Faster deployment

❌ Cons:

Containers depend on host OS kernel (Linux kernel)

🔥 Main Difference Table
Feature	VM	Docker
Startup time	Minutes	Seconds
Size	GBs	MBs
OS	Full OS inside	Shares host kernel
Performance	Lower	Higher
Isolation	Strong	Strong but lighter
3) Understanding Docker Architecture
What gets installed when Docker is installed?

When you install Docker, these components are installed:

1️⃣ Docker Client

Command you use: docker

Example: docker ps, docker run

2️⃣ Docker Daemon (dockerd)

Runs in background

Creates and manages containers, images, networks, volumes

3️⃣ Docker Engine

Combination of client + daemon

4️⃣ Container Runtime (containerd + runc)

Responsible for actually running containers

5️⃣ Docker Images & Containers

Image = template

Container = running instance

🔁 How it Works

You run:

docker run nginx


Flow:

Docker client sends request

Docker daemon pulls image

Container runtime runs container

Container runs isolated process

4) Dockerfile Deep Dive (Explain each line)
Example Dockerfile (Node App)
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]

Line by Line Explanation
✅ FROM node:18-alpine

Base image used

Alpine is lightweight Linux

✅ WORKDIR /app

Sets working directory inside container

✅ COPY package*.json ./

Copies package.json and package-lock.json

Helps caching

✅ RUN npm install

Runs command during build time

Installs dependencies

✅ COPY . .

Copies full source code

✅ EXPOSE 3000

Documentation that container uses port 3000

Does NOT publish the port automatically

✅ CMD ["npm", "start"]

Default command when container starts

5) Key Docker Commands
🐳 Basic Commands
Command	Use

docker --version	Check Docker version
docker images	List images
docker ps	Running containers
docker ps -a	All containers
docker pull nginx	Download image
docker run nginx	Run container
docker stop <id>	Stop container
docker rm <id>	Remove container
docker rmi <image>	Remove image

🔍 Debugging Commands
Command	Use
docker logs <container>	Show logs
docker exec -it <container> bash	Enter container
docker inspect <container>	Full info
docker stats	CPU/RAM usage
7) Docker Networking

Docker provides networking so containers can communicate.

Types of Docker Networks
1️⃣ Bridge Network (Default)

Containers communicate inside same network

Used mostly for single host

docker network ls
docker network create my-net

2️⃣ Host Network

Container uses host network directly

No isolation

3️⃣ None Network

No networking at all

4️⃣ Overlay Network

🐳 Docker Deep Dive – Super Clear Explanation
1️⃣ What Problem Does Docker Solve? (VERY IMPORTANT)
❌ Problem Before Docker

Imagine this situation:

Developer laptop → App works

Testing server → App fails

Production → App crashes

Why?

Because:

Different OS versions

Different library versions

Missing dependencies

Manual setup steps

Example:

“My app needs Node 18, but server has Node 14”

✅ What Docker Does

Docker puts everything needed to run the app into one box called a container.

That box includes:

App code

Runtime (Node, Java, Python)

Libraries

Config

📦 One box → runs same everywhere

Laptop = Server = Cloud

🔥 One-line Interview Answer

Docker solves the problem of environment inconsistency by packaging applications and their dependencies into containers that run the same across all systems.

2️⃣ Virtual Machines vs Docker (VERY CLEAR)
🖥 Virtual Machine (VM)

VM = Computer inside a computer

Structure:

Hardware
Host OS
Hypervisor
Guest OS
Application


Problems:

Full OS inside OS

Heavy (GBs)

Slow startup (minutes)

🐳 Docker Container

Docker = Process isolation, not full OS

Structure:

Hardware
Host OS
Docker
Container (App + libs)


Benefits:

No extra OS

Lightweight

Starts in seconds

🧠 Simple Analogy

VM → Renting a full house 🏠

Docker → Renting a room 🛏️


3️⃣ Docker Architecture (What Gets Installed?)

When you install Docker, you install 5 things.

1️⃣ Docker Client

Command you type

docker run nginx

2️⃣ Docker Daemon (dockerd)

Background service

Does the real work

Creates containers, images, networks

3️⃣ Docker Engine

Client + Daemon together

4️⃣ Container Runtime

Runs the container process

Uses Linux features (namespaces, cgroups)

5️⃣ Docker Registry

Place where images are stored

Example:

Docker Hub

GitHub Container Registry

🔁 Flow (Very Important)
docker run nginx
↓
Docker Client
↓
Docker Daemon
↓
Pull Image
↓
Create Container
↓
Run App

4️⃣ Dockerfile – Line by Line (Crystal Clear)
What is Dockerfile?

A recipe to build a Docker image.

Example Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]

Explanation (One by One)
🔹 FROM
FROM node:18-alpine


Base image

Gives Node + Linux

🔹 WORKDIR
WORKDIR /app


Creates /app

All commands run here

🔹 COPY
COPY package.json .


Copy file from local → container

🔹 RUN
RUN npm install


Runs during image build

Installs dependencies

🔹 COPY . .

Copies source code

🔹 EXPOSE
EXPOSE 3000


Says app listens on port 3000

Documentation only

🔹 CMD
CMD ["npm", "start"]


Command when container starts

🔥 Interview Tip

RUN executes during image build, CMD executes during container start.

5️⃣ Key Docker Commands (Grouped for Clarity)
🔹 Image Commands
docker images
docker pull nginx
docker rmi nginx

🔹 Container Commands
docker run nginx
docker ps
docker stop <id>
docker rm <id>

🔹 Debug Commands
docker logs <id>
docker exec -it <id> bash

6️⃣ Docker Networking (Simple)
Why Networking?

Containers must talk to each other.

Example:

Strapi → Postgres

Default Bridge Network

Containers get private IPs

Can talk using container name

Example:

DATABASE_HOST=postgres

Types of Networks
Type	Use
bridge	Default
host	Uses host network
none	No network
overlay	Multi-host

7️⃣ Volumes & Persistence (VERY IMPORTANT)
❌ Problem

Containers are temporary.

If container stops → data lost.

✅ Solution: Volumes

Volumes store data outside container.

Example
volumes:
  postgres_data:

postgres:
  volumes:
    - postgres_data:/var/lib/postgresql/data

