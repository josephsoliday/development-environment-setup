# Development Environment Setup
  
## Using Visual Studio with Ubuntu Subsystem
  
Before beginning, please install [Visual Studio Code](https://code.visualstudio.com/) for Windows.
  
### Ubuntu WSL Setup
  
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
  
### WSL Setup
  
1. Install the [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) extension in VS Code
2. Access Linux file system using wsl$ (add to quick access) when cloning repos
  
**NOTE:** Please see
[WSL VS Code Tutorial](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode) for more details.
