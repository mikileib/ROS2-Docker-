
# Setting Up ROS 2 in Docker with Foxglove Studio

This tutorial guides you through setting up ROS 2 inside a Docker container and using Foxglove Studio to visualize ROS 2 topics. This setup allows you to run ROS 2 on macOS using Docker.

## Prerequisites

1. **Docker**: Ensure Docker is installed on your macOS.
   - You can download and install Docker from [Docker's official site](https://www.docker.com/products/docker-desktop).

2. **Foxglove Studio**: Install Foxglove Studio on your macOS.
   - Download it from [Foxglove Studio's website](https://foxglove.dev/download).

## Steps to Set Up ROS 2 in Docker

### 1. Pull the ROS 2 Docker Image

Open your terminal and pull the ROS 2 Humble desktop image:

```sh
docker pull osrf/ros:humble-desktop
```

### 2. Start the ROS 2 Container

Run the Docker container with port forwarding to expose the WebSocket server:

```sh
docker run -it --name=ros2 -p 8765:8765 osrf/ros:humble-desktop bash
```

### 3. Install and Run `rosbridge_server`

Inside the Docker container, update package lists and install `rosbridge_server`:

```sh
apt update
apt install ros-humble-rosbridge-server
```

Start the `rosbridge_websocket` server:

```sh
ros2 launch rosbridge_server rosbridge_websocket_launch.xml
```

### 4. Publish a ROS 2 Topic

Open another terminal and connect to the running container:

```sh
docker exec -it ros2 ./ros_entrypoint.sh bash
```

Run a demo node to publish a topic:

```sh
ros2 run demo_nodes_cpp talker
```

### 5. Connect Foxglove Studio to ROS 2

1. Open Foxglove Studio on your macOS.
2. Add a new connection by clicking on the "+" button.
3. Select "ROS 2 WebSocket" as the connection type.
4. Enter `ws://localhost:8765` as the WebSocket URL.
5. Click "Connect".

### 6. Verify the Connection

In Foxglove Studio:
- Add a new panel (e.g., "Raw Messages").
- Select the `/chatter` topic (or any other topic you have published).
- You should see the messages being published from the ROS 2 container.
