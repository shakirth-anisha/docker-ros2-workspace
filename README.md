# ROS 2 Docker Workspace

This repository provides a base workspace structure for developing ROS 2 applications using Docker Compose. It is designed to give you a consistent, reproducible development environment across machines without installing ROS 2 directly on the host.

---

## Table of Contents
- [Overview](#ros-2-docker-workspace)
- [Repository structure](#repository-structure)
- [Prerequisites](#prerequisites)
- [Get started](#get-started)
- [Working with the container](#working-with-the-container)
- [Switch ROS 2 distributions](#switch-ros-2-distributions)

---

## Repository structure

Use `ws_linux` or `ws_mac` according to your operating system. Each workspace handles display forwarding and mounting differently, hence the separate workspace directories.

```text
ws_linux/
├── compose.yml          # Docker Compose configuration
├── Dockerfile           # Container image definition
└── src/                 # Your ROS 2 packages go here

ws_mac/
├── compose.yml          # Docker Compose configuration
├── Dockerfile           # Container image definition
└── src/                 # Your ROS 2 packages go here
```

Build artifacts (`build/`, `install/`, `log/`) are stored in a Docker volume for persistence and performance.

---

## Prerequisites

Before you use this repository, make sure you have:

* Docker installed (with Docker Compose)
* For Linux: X11 display server
* For macOS: XQuartz installed

Basic familiarity with ROS 2 and Docker concepts is recommended.

---

## Get started

### Linux

1. Clone the repository:

   ```bash
   git clone https://github.com/shakirth-anisha/docker-ros2-workspace
   cd docker-ros2-workspace/ws_linux
   ```

2. Allow Docker to access your X server (for GUI applications):

   ```bash
   xhost +local:docker
   ```

3. Start the container:

   ```bash
   docker compose up -d
   ```

4. Enter the container:

   ```bash
   docker compose exec ros2 /bin/bash
   ```

5. Verify the environment:

   ```bash
   echo $ROS_DISTRO
   ```

### macOS

1. Clone the repository:

   ```bash
   git clone https://github.com/shakirth-anisha/docker-ros2-workspace
   cd docker-ros2-workspace/ws_mac
   ```

2. Install and configure XQuartz:

   ```bash
   brew install --cask xquartz
   ```

   After installation, configure XQuartz to allow network clients and enable TCP listening. Restart your computer for the changes to take effect.

   > [!IMPORTANT]
   > **macOS GUI Application Requirements**
   >
   > For GUI applications like turtlesim to work from Docker containers, XQuartz must be configured to:
   > - Allow connections from network clients
   > - Enable TCP listening (set `nolisten_tcp=false`)
   >
   > Without these settings, GUI apps will fail with "Authorization required" or "could not connect to display" errors.

3. After reboot, enable TCP listening and allow connections:

   ```bash
   defaults write org.xquartz.X11 nolisten_tcp -bool false
   xhost +localhost
   xhost + 127.0.0.1
   ```

4. Start the container:

   ```bash
   docker compose up -d
   ```

5. Enter the container:

   ```bash
   docker compose exec ros2 /bin/bash
   ```

6. Verify the environment:

   ```bash
   echo $ROS_DISTRO
   ```

---

## Working with the container

### Build your ROS 2 packages

Inside the container:

```bash
cd /home/ws
colcon build
source install/setup.bash
```

### Open additional terminal sessions

From your host machine:

```bash
docker compose exec ros2 /bin/bash
```

### Stop the container

```bash
docker compose stop
```

### Stop and remove the container

```bash
docker compose down
```

### Remove the container and build cache

```bash
docker compose down -v
```

---

## Switch ROS 2 distributions

To use a different ROS 2 distribution (such as Jazzy or Rolling):

1. Edit the `Dockerfile` and change the base image:

   ```dockerfile
   FROM ros:jazzy
   ```

2. Rebuild the image:

   ```bash
   docker compose build
   ```

3. Restart the container:

   ```bash
   docker compose up -d
   ```

> [!NOTE]
> If you want to start with a clean build cache, remove the volume first with `docker compose down -v`.

---
