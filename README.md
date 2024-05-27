# Development Environment Setup

This is my development environment setup.
  
## Visual Studio Code Setup for WSL

This section will outline how I setup my Visual Studio Code IDE to use the Windows Subsystem for Linux (WSL).

### Prerequisites

* [Visual Studio Code](https://code.visualstudio.com/) for Windows
  
### WSL
  
1. Install the Ubuntu app in the Microsoft Store
2. Use "Turn Windows features on or off" and select "Windows Subsystem for Linux", click OK, reboot, and use Ubuntu


#### Upgrade to WSL 2

1. Install the [WSL 2 update](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
2. Open Windows PowerShell
3. Set default version:
    ```
    wsl --set-default-version 2
    ```
4. Upgrade Ubuntu distribution to WSL 2:
    ```
    wsl --set-version Ubuntu 2
    ```
5. Fix DNS resolution issues by doing the following:
    Create a file called `reset-resolvconf.sh` in your `/usr/local/bin/reset-resolvconf.sh` with the following content
    ```
    #!/usr/bin/env bash

    # Remove existing "nameserver" lines from /etc/resolv.conf
    sed -i '/nameserver/d' /etc/resolv.conf
    
    # Run the PowerShell command to generate "nameserver" lines and append to /etc/resolv.conf
    # we use full path here to support boot command with root user
    /mnt/c/Windows/System32/WindowsPowerShell/v1.0/powershell.exe -Command '(Get-DnsClientServerAddress -AddressFamily IPv4).ServerAddresses | ForEach-Object { "nameserver $_" }' | tr -d '\r'| tee -a /etc/resolv.conf > /dev/null

    ```
   **NOTE:** If you need to connect to VPN often, try this approach: https://gist.github.com/ThePlenkov/6ecf2a43e2b3898e8cd4986d277b5ecf

    Change the execution of the reset-resolvconf.sh file:
    ```
    sudo chmod 777 /usr/local/bin/reset-resolvconf.sh
    ```
    Create a file under /etc/sudoers.d/ to allow no password when running reset-resolvconf.sh with sudo:
    ```
    sudo vi /etc/sudoers.d/wsl_custom
    ```
    Add the No Password Rule:
    ```
    your_username ALL=(ALL) NOPASSWD: /usr/local/bin/reset-resolvconf.sh
    ```
    Add an alias to you `~/.bashrc` file:
    ```
    alias reset-resolvconf="sudo /usr/local/bin/reset-resolvconf.sh"
    ```
    Optionally, add a line to start reset-resolvconf.sh in your `~/.bashrc` to run at startup:
    ```
    sudo /usr/local/bin/reset-resolvconf.sh
    ```
7. Open Docker desktop, click on **Resources->WSL INTEGRATION** and enable Ubuntu
8. Remove the **export DOCKER_HOST** entry from .bashrc
9. Upgrade docker on Ubuntu:
    ```
    sudo apt install docker.io
    ```
  
### Docker
  
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
5. Configure the docker daemon to connect to Docker (not needed for WSL 2):
    ```
    echo "export DOCKER_HOST='tcp://0.0.0.0:2375'" >> ~/.bashrc
    source ~/.bashrc
    ```
  
### Git
  
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
5. Configure username and email:
    ```
    git config --global user.name "<username>"
    git config --global user.email "<email>"
    ```

### Python

1. Install [Python for Windows](https://www.python.org/downloads/windows/)
2. Install/Upgrade [Python for Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-programming-environment-on-an-ubuntu-20-04-server)

### Java

1. Install [JDK 11 for Windows](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html)
2. Install [JDK 11 for Ubuntu](https://computingforgeeks.com/how-to-install-java-11-on-ubuntu-debian-linux/)

### Maven

1. Install [Maven for Windows](http://maven.apache.org/download.cgi)
2. Install [Maven for Ubuntu](https://tecadmin.net/install-apache-maven-ubuntu-20-04/)

### Gradle

1. Install [Gradle for Windows](https://gradle.org/install/)
2. Install [Gradle for Ubuntu](https://tecadmin.net/install-gradle-ubuntu-20-04/)

### Kubernetes

1. Install [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

### Spark

1. Download Apache Spark and move it to `/usr/local/spark`:
    ``` bash
    wget https://apache.mirror.digionline.de/spark/spark-2.4.7/spark-2.4.7-bin-hadoop2.7.tgz
    tar xvf spark-2.4.7-bin-hadoop2.7.tgz
    sudo mv spark-2.4.7-bin-hadoop2.7 /usr/local/spark
    ```
2. Edit `~/.bashrc` using `vi` and add the following lines at the end of the file:
    ``` bash
    export SPARK_HOME=/usr/local/spark
    export HADOOP_HOME=/usr/local/spark
    export PYTHONPATH=$SPARK_HOME/python:$SPARK_HOME/python/lib/py4j-0.10.7-src.zip:$PYTHONPATH
    export PATH=$SPARK_HOME/bin:$SPARK_HOME/python:$PATH
    ```
3. Activate changes:
    ``` bash
    source ~/.bashrc
    ```
4. Install pyspark:
    ``` bash
    pip install pyspark
    ```

### Kafka

1. Create a docker-compose.yml with the following content: [docker-compose.yml](https://github.com/josephsoliday/kafka/blob/main/docker-compose.yml)
2. Start Kafka using docker-compose:
    ```bash
    docker-compose up -d
    ```
3. Install the [Tools for Apache Kafka](https://marketplace.visualstudio.com/items?itemName=jeppeandersen.vscode-kafka) extension

### Extensions
  
1. Install the [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) extension in VS Code
2. Access Linux file system using wsl$ (add to quick access) when cloning repos
3. In case of issues with the Linux file system, you can launch Visual Studio Code using the Ubuntu File System:
   1. Open the Ubuntu prompt
   2. Run Visual Studio Code:
        ```
        code .
        ```
   3. Click on Extensions
   4. Scroll down the **LOCAL - INSTALLED** list and install the extensions on Ubuntu
  
**NOTE:** Please see
[WSL VS Code Tutorial](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode) for more details.

## IntelliJ Setup for WSL

This section will outline how I setup my IntelliJ IDE to use the Windows Subsystem for Linux (WSL).

1. Install [XLaunch](https://sourceforge.net/projects/vcxsrv/)

2. Create a shortcut for XLaunch to startup automatically using the following `Target` field:
    ```
    "C:\Program Files\VcXsrv\vcxsrv.exe" :0 -multiwindow -clipboard -wgl -ac
    ```

3. Move the shortcut to the `%AppData%\Microsoft\Windows\Start Menu\Programs\Startup` folder and launch it

4. Download IntelliJ on WSL:
    ``` bash
    cd /usr/local
    wget https://download.jetbrains.com/idea/ideaIU-2020.2.3.tar.gz
    tar -zxf ideaIU-2020.2.3.tar.gz
    mv idea-IU-202.7660.26 idea
    ```

5. Create a desktop shortcut with the following `Target` field:
    ```
    %SystemRoot%\System32\bash.exe -c "cd && DISPLAY=:0 /usr/local/idea/bin/idea.sh"
    ```

6. Pin the shortcut to your taskbar and optionally change the icon

7. Launch IntelliJ
