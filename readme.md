# Deploy ASP.NET 5 Apps
## To a Linux Virtual Machine with Docker

1. Install **Docker** on Linux Ubuntu

  - Install Docker from its own package source:

    ```
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
    sudo sh -c "echo deb https://get.docker.com/ubuntu docker main > /etc/apt/sources.list.d/docker.list"
    sudo apt-get update
    sudo apt-get install lxc-docker
    ```
  - To verify Docker installation enter: `docker --version`
  - Eliminate need to prefix docker commands with `sudo` for root privilges

    ```
    sudo groupadd docker
    sudo gpasswd -a parallels docker
    sudo service docker restart
    newgrp docker
    ```
  - To verify docker root privileges enter: `docker run --help`
  - Say hello to Docker: `docker run hello-world`
    + You should see the message: `Hello from Docker`

2. Run **console sample**
  - Create ConsoleApp folder
  - Go to the aspnet repo: https://github.com/aspnet/Home/tree/dev/samples/latest/ConsoleApp
  - Download: project.json, program.cs
  - Add a file called `Dockerfile` in the app directory with the following contents:

    ```
    FROM microsoft/aspnet:1.0.0-beta4
    COPY . /app
    WORKDIR /app
    RUN ["dnu", "restore"]
    
    ENTRYPOINT ["dnx", ".", "run"]
    ```
  - From the app directory, build the docker image

    ```
    docker build -t consoleapp .
    ```
  - To verify enter: `docker images`
  - Run the container:

    ```
    docker run -t consoleapp
    ```
  - You should see the followint output: `Hello World`
  
3. Run **web sample**
  - Create WebApp folder
  - Go to the aspnet repo: https://github.com/aspnet/Home/tree/dev/samples/latest/HelloWeb
  - Download: project.json, startup.cs
  - Add an empty directory called `wwwroot`
  - Add a file called `Dockerfile` in the app directory with the following contents:

    ```
    FROM microsoft/aspnet:1.0.0-beta4
    COPY . /app
    WORKDIR /app
    RUN ["dnu", "restore"]
    
    EXPOSE 5004
    ENTRYPOINT ["dnx", ".", "kestrel"]
    ```
  - From the app directory, build the docker image

    ```
    docker build -t webapp .
    ```
  - To verify enter: `docker images`
  - Run the container:

    ```
    docker run -t -d -p 5004:5004 webapp
    ```
  - You'll get back a container id, which is a big long number you can use to read the container logs, which will show generated output, incuding runtime error information.
  - To verify enter: `docker ps`
  - Open a browser and go to: `http://localhost:5004`

