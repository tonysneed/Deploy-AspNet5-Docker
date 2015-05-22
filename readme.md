# Visual Studio Code Setup
## For ASP.NET 5 on Linux Ubuntu

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
    
2. Install **ASP.NET 5** on Linux Ubuntu

  - Instructions: http://docs.asp.net/en/latest/getting-started/installing-on-linux.html
  - Install Mono:
    ```
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
    echo "deb http://download.mono-project.com/repo/debian wheezy main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list
    sudo apt-get update
    sudo apt-get install Mono-Complete
    ```
  - Install libuv
    ```
    sudo apt-get install automake libtool curl
    curl -sSL https://github.com/libuv/libuv/archive/v1.4.2.tar.gz | sudo tar zxfv - -C /usr/local/src
    cd /usr/local/src/libuv-1.4.2
    sudo sh autogen.sha
    sudo ./configure
    sudo make
    sudo make install
    sudo rm -rf /usr/local/src/libuv-1.4.2 && cd ~/
    sudo ldconfig
    ```
  - Install DotNet Version Manager: `dnvm`
    `curl -sSL https://raw.githubusercontent.com/aspnet/Home/dev/dnvminstall.sh | DNX_BRANCH=dev sh && source ~/.dnx/dnvm/dnvm.s`
  - Enter: `source /home/parallels/.dnx/dnvm/dnvm.sh`
  - Run `dnvm`
  - Enter: `dnvm upgrade`

3. Run **console sample**
  - Create ConsoleApp folder
  - Go to the aspnet repo: https://github.com/aspnet/Home/tree/dev/samples/latest/ConsoleApp
  - Download: project.json, program.cs
  - Restore packages: `dnu restore`
  - Run the app: `dnx . run`
  
4. Run **web sample**
  - Create WebApp folder
  - Go to the aspnet repo: https://github.com/aspnet/Home/tree/dev/samples/latest/HelloWeb
  - Download: project.json, startup.cs
  - Restore packages: `dnu restore`
  - Run the app: `dnx . kestrel`
  - Open a browser and go to: http://localhost:5004
  - Try stopping kestrel by pressing Enter

5. Scaffold an **ASP.NET Web API** app using **Yeoman**
  - Install the node package manager: `npm`:

    `sudo apt-get install nodejs-legacy npm`
  - Install Yeoman, the asp.net generator, grunt and bower:
  
    `sudo npm install -g yo grunt-cli generator-aspnet bower`
  - Go to parent folder in Terminal and run `yo aspnet`
    + Pick Web API Application, enter as name: **HelloWebApi**
  - Run `dnu restore`, then run as before: `dnx . kestrel`
  - Open a browser and go to: `http://localhost:5001/api/values`
  - Press Enter to stop Kestrel
