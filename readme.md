# Visual Studio Code Setup
## For ASP.NET 5 on Linux Ubuntu with Docker

1. Download and install **Visual Studio Code**

  - Instructions: https://code.visualstudio.com/Docs/setup
  - Under Home, create a folder called **VSCode**.
  - Extract downloaded zip file to this location
  - Create a link to launch Code from Terminal by typing `code .`
    ```
    sudo ln -s /home/parallels/VSCode/Code /usr/local/bin/code
    ```
  - Create a folder under Home called **Source**
    + Navigate to Source in Terminal and enter:  `code .`
    + VS Code will open at this location
    
2. Install **Docker** on Linux Ubuntu

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
    sudo gpasswd -a ${USER} docker
    sudo service docker restart
    newgrp docker
    ```
  - To verify docker root privileges enter: `docker run --help`
  - Say hello to Docker: `docker run hello-world`
    + You should see the message: `Hello from Docker`

3. Run **console sample**
  - Create ConsoleApp folder
  - Go to the aspnet repo: https://github.com/aspnet/Home/tree/dev/samples/latest/ConsoleApp
  - Download: project.json, program.cs
  - Add a file called `Dockerfile` in the app directory with the following contents:
    ```
    FROM microsoft/aspnet:1.0.0-beta4
    ADD . /app
    WORKDIR /app
    RUN ["dnu", "restore"]
    
    ENTRYPOINT ["dnx", "run"]
    ```
  - From the app directory, build the docker image
    ```
    docker build -t consoleapp .
    ```
  - Run the container:
    ```
    docker run -t -d consoleapp
    ```
  
4. Run **web sample**
  - Create WebApp folder
  - Go to the aspnet repo: https://github.com/aspnet/Home/tree/dev/samples/latest/HelloWeb
  - Download: project.json, startup.cs
  - Add a file called `Dockerfile` in the app directory with the following contents:
    ```
    FROM microsoft/aspnet:1.0.0-beta4
    ADD . /app
    WORKDIR /app
    RUN ["dnu", "restore"]
    
    EXPOSE 5004
    ENTRYPOINT ["dnx", "kestrel"]
    ```
  - From the app directory, build the docker image
    ```
    docker build -t webapp .
    ```
  - Run the container:
    ```
    docker run -t -d -p 5004:5004 webapp
    ```
  - Open a browser and go to: `http://localhost:5004`

