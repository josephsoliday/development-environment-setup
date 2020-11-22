# Development Environment Setup

This is my development environment setup.
  
## Visual Studio Code in WSL

This section will outline how I setup my Visual Studio Code IDE to use the Windows Subsystem for Linux (WSL).

### Prerequisites

* [Visual Studio Code](https://code.visualstudio.com/) for Windows
  
### WSL Setup
  
1. Install the Ubuntu app in the Microsoft Store
2. Use "Turn Windows features on or off" and select "Windows Subsystem for Linux", click OK, reboot, and use Ubuntu 
  
### Docker Setup
  
1. Install [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)
2. Open the settings for Docker and under General check the **Expose daemon on tcp://localhost:2375 without TLS** option
3. Install docker.io:
    ```
    sudo apt update
    sudo apt upgrade
    sudo apt install docker.io
    ```
4. In case of errors run:
    ```
    sudo dpkg -i /var/cache/apt/archives/*.deb
    ```
5. Configure the docker daemon to connect to Docker:
    ```
    echo "export DOCKER_HOST='tcp://0.0.0.0:2375'" >> ~/.bashrc
    source ~/.bashrc
    ```
  
### Git Setup
  
1. Install [Git for Windows](https://gitforwindows.org/)
2. Configure git to keep line endings:
    ``` bash
    git config --global core.autocrlf input
    ```
3. Add the following setting to the settings.json in VS Code: "files.eol": "\n"
4. Install git for Linux:
    ```
    sudo apt install git
    ```
### Python Setup

1. Install [Python for Windows](https://www.python.org/downloads/windows/)
2. Install/Upgrade [Python for Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-programming-environment-on-an-ubuntu-20-04-server)

### Java Setup

1. Install [JDK 11 for Windows](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html)
2. Install [JDK 11 for Ubuntu](https://computingforgeeks.com/how-to-install-java-11-on-ubuntu-debian-linux/)

### Maven Setup

1. Install [Maven for Windows](http://maven.apache.org/download.cgi)
2. Install [Maven for Ubuntu](https://tecadmin.net/install-apache-maven-ubuntu-20-04/#:~:text=%20How%20to%20Install%20Apache%20Maven%20on%20Ubuntu,You%20have%20successfully%20installed%20and%20configured...%20More%20)

### Extensions Setup
  
1. Install the [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) extension in VS Code
2. Access Linux file system using wsl$ (add to quick access) when cloning repos
3. In case of issues with the Linux file system, you can launch Visual Studio Code using the Ubuntu File System:
   1. Open the Ubuntu prompt
   2. Run Visual Studio Code:
        ```bash
        code .
        ```
   3. Click on Extensions
   4. Scroll down the **LOCAL - INSTALLED** list and install the extensions on Ubuntu
  
**NOTE:** Please see
[WSL VS Code Tutorial](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode) for more details.
