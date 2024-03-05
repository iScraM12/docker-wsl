# Installationsanleitung:

## -------------Ubuntu WSL installieren--------------

### Powershell als Admin ausführen
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
wsl.exe --install -d Ubuntu
wsl.exe --set-default-version 2
```

### In Ubuntu WSL Fenster:

#### Username vergeben
#### Passwort vergeben

### Neustart

## -------------Docker sauber installieren und konfigurieren--------------

```
sudo apt update && sudo apt upgrade
sudo apt remove docker docker-engine docker.io containerd runc
sudo apt install --no-install-recommends apt-transport-https ca-certificates curl gnupg2
```

```
sudo update-alternatives --config iptables
```

### -> 1 -> Enter
```
. /etc/os-release
curl -fsSL https://download.docker.com/linux/${ID}/gpg | sudo tee /etc/apt/trusted.gpg.d/docker.asc
```

```
echo "deb [arch=amd64] https://download.docker.com/linux/${ID} ${VERSION_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/docker.list
sudo apt update
```

```
sudo apt install docker-ce docker-ce-cli containerd.io
sudo usermod -aG docker $USER
```

## Ubuntu WSL schließen und wieder öffnen:
```
sudo dockerd
```

## In neuer WSL:

```
docker run --rm hello-world
```

## -------------Docker in Windows und ohne sudo nutzbar machen--------------

```
cd /home/username
sudo nano .bashrc
```
### -> ganz unten einfügen:

```
DOCKER_DISTRO="Ubuntu"
DOCKER_LOG_DIR=$HOME/docker_logs
mkdir -pm o=,ug=rwx "$DOCKER_LOG_DIR"
/mnt/c/Windows/System32/wsl.exe -d $DOCKER_DISTRO sh -c "nohup sudo -b dockerd < /dev/null > $DOCKER_LOG_DIR/dockerd.log 2>&1"
```

### ctrl+x -> y

```
sudo visudo
```
### -> ganz unten einfügen:

```
%docker ALL=(ALL)  NOPASSWD: /usr/bin/dockerd
```

### ctrl+x -> y

## -------------JDK 17 ind WSL installieren--------------

```
sudo apt update
sudo apt upgrade
```

```
apt-cache search openjdk | grep openjdk-17
sudo apt install openjdk-17-jdk
```


## -------------MAVEN in WSL installieren--------------

```
wget https://dlcdn.apache.org/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz
sudo tar xzf apache-maven-3.9.4-bin.tar.gz -C /opt
sudo ln -s /opt/apache-maven-3.9.4 /opt/maven
```

```
sudo nano /etc/profile.d/maven.sh
```

### -> einfügen

```
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export M2_HOME=/opt/maven
export MAVEN_HOME=/opt/maven
export PATH=${M2_HOME}/bin:${PATH}
```

### ctrl+x -> y

```
source /etc/profile.d/maven.sh
mvn -version
rm -f apache-maven-3.8.6-bin.tar.gz
``` 

## -------------Node und NPM ind WSL installieren--------------

```
sudo apt-get install nodejs
sudo apt install npm
```

## -------------Minikube in WSL installieren--------------

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube start
minikube addons enable ingress
minikube addons enable ingress-dns
minikube dashboard
```

### (Fehler von Dashboard ist ok)

```
minikube stop
```

## -------------kubectl in WSL installieren--------------

```
curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update
sudo apt install kubectl
```


