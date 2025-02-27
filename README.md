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
    * Create the `/usr/local/bin/reset-resolvconf.sh` file with the following content:
      ```
      #!/usr/bin/env bash
  
      # Remove existing "nameserver" lines from /etc/resolv.conf
      sed -i '/nameserver/d' /etc/resolv.conf
      
      # Run the PowerShell command to generate "nameserver" lines and append to /etc/resolv.conf
      # we use full path here to support boot command with root user
      /mnt/c/Windows/System32/WindowsPowerShell/v1.0/powershell.exe -Command '(Get-DnsClientServerAddress -AddressFamily IPv4).ServerAddresses | ForEach-Object { "nameserver $_" }' | tr -d '\r'| tee -a /etc/resolv.conf > /dev/null
  
      ```
    * Change the execution of the reset-resolvconf.sh file:
      ```
      sudo chmod 777 /usr/local/bin/reset-resolvconf.sh
      ```
    * Create a file under /etc/sudoers.d/ to allow no password when running reset-resolvconf.sh with sudo:
      ```
      sudo vi /etc/sudoers.d/wsl_custom
      ```
    * Add the No Password Rule:
      ```
      your_username ALL=(ALL) NOPASSWD: /usr/local/bin/reset-resolvconf.sh
      ```
    * Add an alias to you `~/.bashrc` file:
      ```
      alias reset-resolvconf="sudo /usr/local/bin/reset-resolvconf.sh"
      ```
    * Optionally, add a line to start reset-resolvconf.sh in your `~/.bashrc` to run at startup:
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
2. Install Python for Ubuntu:
    ```
    sudo apt-get update; sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev libncursesw5-dev libgdbm-dev libc6-dev libssl-dev openssl bzip2 libbz2-dev libreadline-dev libsqlite3-dev
    wget -q https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer -O- | bash
    echo 'export PATH="$HOME/.pyenv/bin:$PATH"' >> ~/.bashrc
    echo 'eval "$(pyenv init --path)"' >> ~/.bashrc
    echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc
    ```
    Restart Ubuntu and run the following commands to install Python:
    ```
    pyenv install 3.10.0
    pyenv global 3.10.0
    ```
### Java

1. Install [SDKMAN](https://sdkman.io/install/)
2. Install latest version of Java per the instructions
3. Configure method parameters:
   ```bash
   # local
   echo "org.eclipse.jdt.core.compiler.codegen.methodParameters=generate" > .settings/org.eclipse.jdt.core.pref
   # globally
   echo "org.eclipse.jdt.core.compiler.codegen.methodParameters=generate" > ~/.vscode-server/java.prefs
   ```
   Add the following to your settings.json in VS Code:
   ```json
   "java.settings.url": "~/.vscode-server/java.prefs"
   ```
   Clean your Java workspace files in VS Code and restart.
   

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
