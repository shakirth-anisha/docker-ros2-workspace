# ROS 2 Docker Workspace

This repository provides a base workspace structure for developing ROS 2 applications using Docker and development containers. It is designed to give you a consistent, reproducible development environment across machines without installing ROS 2 directly on the host.

---

## Table of Contents
- [Overview](#ros-2-docker-workspace)
- [Repository structure](#repository-structure)
- [Prerequisites](#prerequisites)
- [Get started](#get-started)
- [Switch ROS 2 distributions](#switch-ros-2-distributions)

---

## Repository structure
Use `ws_linux` or `ws_mac` according to your operating system. Each workspace handles display forwarding and mounting differently, hence the separate workspace directories.

```text
ws/
├── cache/
│   └── humble/
│       ├── build/
│       ├── install/
│       └── log/
└── src/
    └── .devcontainer/
        ├── Dockerfile
        └── devcontainer.json
````

The `cache` directory stores build artifacts for each ROS 2 distribution. This lets you switch distributions without rebuilding everything from scratch.

The `src` directory contains your ROS 2 packages and the development container configuration.

---

## Prerequisites

Before you use this repository, make sure you have:

* Docker installed
* Visual Studio Code installed
* Dev Containers extension for Visual Studio Code

Basic familiarity with ROS 2 and Docker concepts is recommended.

---

## Get started

1. Clone the repository:

   ```bash
   git clone https://github.com/shakirth-anisha/docker-ros2-workspace
   cd ros2-docker-workspace
    ```

2. Start and build devcontainer:

    ```bash
    devcontainer up --workspace-folder ws
    devcontainer exec --workspace-folder ws/src colcon build --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=ON
    ```

3. Open the workspace in Visual Studio Code:

   * Open the `src` directory
   * When prompted, select **Reopen in Container**

   Visual Studio Code builds the Docker image and opens the workspace inside the container.

4. Verify the environment:

   ```bash
   ros2 --version
   which colcon
   ```

---

## Switch ROS 2 distributions

To use a different ROS 2 distribution:

1. Create cache directories for the distribution.
2. Rename `humble` in devcontainer.json and Dockerfile to the required distribution.
3. Update the base image in the Dockerfile.
4. Update cache mount paths in `devcontainer.json`.
5. Rebuild the container using **Dev Containers: Rebuild Container**.

---
