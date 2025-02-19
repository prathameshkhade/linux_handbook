## 13.2.2 Docker Fundamentals: Getting Your Hands Dirty
Hey there! Ever felt like your computer is a bit of a chaotic mess? Files scattered, applications clashing, and setting up a new project feels like navigating a jungle? Yeah, I’ve been there. But guess what? There’s a way to bring order to this digital wilderness, and it’s all thanks to something called **containers**.

Think of containers as lightweight, portable boxes for your applications. Imagine packing up everything your app needs – the code, the libraries, the settings – into one neat box. This box, a container, can then be shipped anywhere and run consistently, no matter the environment. Pretty cool, right?

My journey into the world of containers started with **Docker**, a super popular tool that made this containerization thing accessible to everyone. Then, I discovered **Podman**, a powerful alternative with some really interesting twists. Let me share what I learned, and hopefully, it’ll help you start your own container adventure!


### 13.2.2.1 Installing Docker: Setting Sail

First things first, we need to get Docker onto our machine. 

#### Installing Docker on Ubuntu: Smooth Sailing

To get Docker up and running on Ubuntu, follow these steps:

1. **Update your package index**:
    ```sh
    $ sudo apt-get update
    ```

2. **Install necessary packages**:
    ```sh
    $ sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
    ```

3. **Add Docker’s official GPG key**:
    ```sh
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

4. **Add Docker’s repository**:
    ```sh
    $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

5. **Update your package index again**:
    ```sh
    $ sudo apt-get update
    ```

6. **Install Docker**:
    ```sh
    $ sudo apt-get install docker-ce
    ```

7. **Verify Docker installation**:
    ```sh
    $ sudo systemctl status docker
    ```

#### Installing Docker on Red Hat Based OS: Anchors Aweigh

For Red Hat-based systems like CentOS or Fedora, here’s how you can install Docker:

1. **Update your package index**:
    ```sh
    $ sudo yum update
    ```

2. **Install necessary packages**:
    ```sh
    $ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
    ```

3. **Add Docker’s repository**:
    ```sh
    $ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    ```

4. **Install Docker**:
    ```sh
    $ sudo yum install docker-ce
    ```

5. **Start Docker**:
    ```sh
    $ sudo systemctl start docker
    ```

6. **Enable Docker to start on boot**:
    ```sh
    $ sudo systemctl enable docker
    ```

7. **Verify Docker installation**:
    ```sh
    $ sudo systemctl status docker
    ```

And there you have it! Docker should now be installed and running on your system, ready to containerize your applications.

Once installed, the real fun begins! Let’s try some basic commands. Open up your terminal – this is our command center.

*   **`docker pull <image_name>`**: This is like downloading an app from an app store, but for containers. Think of an **image** as a template for a container. Let’s pull a simple “hello-world” image:
    
        $ docker pull hello-world
        
    
    You should see Docker downloading layers of the image. These layers are what make containers so efficient – they share common parts, so downloads are quick!
    
*   **`docker run <image_name>`**: Time to actually run our container! Let’s run that “hello-world” image we just pulled:
    
        $ docker run hello-world
        
    
    Boom! You should see a friendly message welcoming you to Docker. This is your first container in action!
    
*   **`docker ps`**: Want to see what containers are currently running? This command is your friend.
    
        $ docker ps
        
    
    If you just ran “hello-world”, you might not see anything, because it’s designed to run and then exit. To see _all_ containers, even those that have stopped, use:
    
        $ docker ps -a
        
    
    You’ll likely see the “hello-world” container listed there, along with its status.
    
*   **`docker images`**: Curious about the images you’ve downloaded? This command shows you all the images stored locally on your machine:
    
        $ docker images
        
    
    You should see “hello-world” in the list.
    
*   **`docker build -t <your_image_name> .`**: We’ll get to building our own images soon, but this is the command we’ll use. The `-t` flag lets you tag your image with a name, making it easier to identify later. The `.` at the end tells Docker to look for the Dockerfile (more on that next) in the current directory.
    

### 13.2.2.2 Building a Simple Container: Creating Our First Box**

Okay, let’s build our own container! Let's say we want to create a simple web server that just says “Hello from Docker!”. For this, we need a **Dockerfile**. Think of a Dockerfile as a recipe for building a container image.

Create a new directory (maybe call it `my-web-app`) and inside it, create a file named `Dockerfile` (no extension!). Inside `Dockerfile`, paste the following:

```yaml
FROM httpd:latest
COPY index.html /usr/local/apache2/htdocs/
EXPOSE 80
```

Let’s break this down:

*   `FROM httpd:latest`: We’re starting with a base image `httpd:latest`, which is an official Docker image containing the Apache web server. Think of it as a pre-built foundation for our web app.
*   `COPY index.html /usr/local/apache2/htdocs/`: This copies a file named `index.html` (which we’ll create next) into the web server’s document root within the container.
*   `EXPOSE 80`: This tells Docker that our container will listen on port 80 (the standard HTTP port).

Now, create a file named `index.html` in the same directory as your `Dockerfile` with the following content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Hello Docker!</title>
</head>
<body>
    <h1>Hello from Docker!</h1>
</body>
</html>
```
    

Now, navigate to your `my-web-app` directory in your terminal and build your image:

    $ docker build -t my-web-image .
    

Once the build is successful, run your container, mapping port 8080 on your computer to port 80 in the container:

    $ docker run -d -p 8080:80 my-web-image
    

*   `-d`: Runs the container in detached mode (in the background).
*   `-p 8080:80`: Maps port 8080 on your host machine to port 80 in the container.

Open your web browser and go to `http://localhost:8080`. You should see “Hello from Docker!” You just built and ran your own web server inside a container!

### 13.2.2.3 Managing Containers: Taking Control

Now that our web server is running, let's learn to manage it.

*   **`docker start <container_id>`**: To start a stopped container. You can find the `container_id` using `docker ps -a`.
*   **`docker stop <container_id>`**: To gracefully stop a running container. It sends a signal to the application inside to shut down cleanly.
*   **`docker restart <container_id>`**: To restart a container. This is a quick way to apply configuration changes or recover from minor issues.
*   **`docker rm <container_id>`**: To delete a stopped container. Be careful, this is permanent!

For example, to stop the web server container we just ran (you’ll need to find its `container_id` using `docker ps`), you would do:

    $ docker stop <container_id>
    

### 13.2.2.4 Networking: Connecting Containers

Containers aren't isolated islands. They often need to talk to each other or the outside world. Docker provides different networking modes.

*   **Bridge Mode (Default):** Containers are connected to a virtual bridge network within the Docker host. They get their own IP address within this network and can communicate with each other and the outside world (through port mapping, like we did with `-p 8080:80`).
*   **Host Mode:** The container shares the host machine's network namespace. This means the container uses the host’s IP address and network interfaces directly. Networking is faster, but it can lead to port conflicts if multiple containers try to use the same port.

Docker also lets you create custom networks and link containers together. For example, you can create a network and then run two containers within it:

    $ docker network create my-network
    $ docker run --name web-app --network my-network -d my-web-image
    $ docker run --name database --network my-network -d <database_image>
    

Now, `web-app` and `database` can communicate with each other using their container names as hostnames within the `my-network`.

### 13.2.2.5 Volumes and Data Persistence: Keeping Data Safe

Containers are designed to be ephemeral – they can be easily created and destroyed. But what if your application needs to store data persistently? That’s where **volumes** come in.

Volumes are directories on your host machine that are mounted into containers. Changes made inside the volume within the container are reflected on the host, and vice-versa. This means data persists even if the container is stopped or deleted.

To mount a volume, use the `-v` flag with `docker run`:

    $ docker run -d -p 8080:80 -v my-web-data:/usr/local/apache2/htdocs/ my-web-image
    

Here, `my-web-data` (which will be created in your current directory if it doesn't exist) on your host machine is mounted to `/usr/local/apache2/htdocs/` inside the container. Any files you put in `my-web-data` on your host will be accessible to the web server in the container.

## 13.2.3 Podman Basics: The Daemonless Challenger

Now, let’s switch gears to **Podman**. Podman is often considered a drop-in replacement for Docker, but with some key differences.

### 13.2.3.1 Installing Podman: A New Captain

Installing Podman is similar to Docker, and there are again great guides online for Ubuntu and RedHat. Just search for “install Podman Ubuntu” or “install Podman RedHat”.

Once installed, you'll notice something interesting: Podman commands are very similar to Docker commands! In many cases, you can just replace `docker` with `podman` and things will work.

### **13.2.3.2 Running Containers with Podman: Familiar Waters**

Let’s try running our “hello-world” image with Podman:

    podman run hello-world
    

See? It’s almost identical to the Docker command! You can use `podman run`, `podman ps`, `podman images`, etc., pretty much the same way you used Docker.

### **13.2.3.3 Key Differences: Daemon vs. Daemonless - The Engine Room**

Here’s the big one: Docker uses a **daemon** architecture. A daemon is a background process that manages Docker containers. You talk to the daemon using the `docker` command-line tool.

Podman, on the other hand, is **daemonless**. It directly interacts with the container runtime (like runc or crun) without a central daemon process.

*   **Docker (Daemon-based):** Client (docker CLI) -> Docker Daemon -> Container Runtime (runc, etc.)
*   **Podman (Daemonless):** Podman CLI -> Container Runtime (runc, etc.)

This daemonless architecture in Podman offers some advantages:

*   **Security:** Without a central daemon running as root, there’s a smaller attack surface. If the daemon is compromised in Docker, it could potentially affect all containers.
*   **Stability:** Daemon restarts or crashes in Docker can impact all running containers. Podman avoids this single point of failure.
*   **Rootless Containers:** Podman really shines with **rootless containers**.

### **13.2.3.4 Rootless Containers: Enhanced Security - Sailing Safer**

Running containers as root (the superuser) can be risky. If a vulnerability is exploited within a root container, it could potentially compromise the entire host system.

Podman is designed to easily run containers **rootless**, meaning as a non-root user. This significantly enhances security, as containers run with limited privileges.

To run a container rootless with Podman, you often don’t need to do anything special – Podman defaults to trying to run containers rootless if possible. However, for certain configurations (like port mapping to privileged ports < 1024), you might need extra configuration or use tools like `podman unshare`.

Rootless containers are a fantastic step forward in container security, and Podman makes them easily accessible.
