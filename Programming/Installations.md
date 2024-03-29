Guide to Install everything you need.
## git

```shell
$ sudo apt install git-all
```

# zsh

```shell
$ sudo apt install zsh
```

## zsh plugins
- ### Powerlevel10k

```shell
$ git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```
	Add/Update ZSH_THEME="powerlevel10k/powerlevel10k" in the ``.zshrc`` file.

- ### zsh-autosuggestions

```shell
$ git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

- ### zsh-syntax-highlighting

```shell
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

## [Docker and Docker Compose](https://docs.docker.com/)

```shell
$ sudo mkdir -p /etc/apt/keyrings
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
$ echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```shell
$ sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
#### Troubleshooting?
[Possible solution](https://github.com/MatAndreoli/Programming/blob/master/Programming/Troubleshooting.md#docker-troubleshooting)

To use docker without sudo, you'll have to add your user to the docker group:
```shell
$ sudo groupadd docker
$ sudo gpasswd -a $USER docker
```

## [nvm](https://github.com/nvm-sh/nvm) - Node Version Manager
```shell
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash
```

Add/Update your ``.zshrc`` file with:
```shell
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

## [Java](https://docs.oracle.com/javase/8/docs/api/)
```shell
$ sudo apt install default-jdk
```

Add/Update your ``.zshrc or /etc/environment`` file with:
```shell
export JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64/"
```

Not sure where java is installed?
```shell
$ update-alternatives --config java
```

## [Maven](https://maven.apache.org/guides/)
```shell
$ sudo apt install maven
```

## [Python](https://docs.python.org/3/)
- ### [Pyenv](https://github.com/pyenv/pyenv) - Python Version Management
```shell
$ curl https://pyenv.run | zsh
```

Add/Update your ``.zshrc`` file with:
```shell
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
```

That's it, now just [start installing python versions.](https://github.com/pyenv/pyenv#install-additional-python-versions)
- ### Virtualenv
```python
$ pip install virtualenv
```

Add/Update your ``.zshrc`` file with:
```shell
eval "$(pyenv virtualenv-init -)"
```

To create a new environment for python:
```shell
$ virtualenv -p python3.8 ~/.venvs/env3.8
```

## [Kubernetes](https://kubernetes.io/docs/home/)
- ### [Minikube](https://kubernetes.io/docs/home/) 
```shell
$ winget install minikube
```

To be able to use minikube in WSL, create a bin with:
```shell
#!/bin/sh
/mnt/c/Program\ Files/Kubernetes/Minikube/minikube.exe $@
```

- ### [Kubectl](https://kubernetes.io/docs/home/) 
```shell
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

$ curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

$ echo "$(cat kubectl.sha256) kubectl" | sha256sum --check
"kubectl: OK"

$ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

- ### [k9s](https://k9scli.io/) 
```shell
$ curl -L https://github.com/derailed/k9s/releases/download/v0.26.7/k9s_Linux_x86_64.tar.gz -o k9s
$ tar -xf k9s
$ sudo chmod +x k9s
$ mv ./k9s /usr/local/bin/k9s
```

- ### Kubectl Krew
```shell
$ (
  set -x; cd "$(mktemp -d)" &&
  OS="$(uname | tr '[:upper:]' '[:lower:]')" &&
  ARCH="$(uname -m | sed -e 's/x86_64/amd64/' -e 's/\(arm\)\(64\)\?.*/\1\2/' -e 's/aarch64$/arm64/')" &&
  KREW="krew-${OS}_${ARCH}" &&
  curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/${KREW}.tar.gz" &&
  tar zxvf "${KREW}.tar.gz" &&
  ./"${KREW}" install krew
)
```

Add this to .zshrc file:
```shell
export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"
```

- ### Kubectx
```shell
$ kubectl krew install ctx
```

- ### Kubens
```shell
$ kubectl krew install ns
```

- ### [Helm](https://helm.sh/)
```shell
$ curl -LO https://get.helm.sh/helm-v3.10.2-linux-amd64.tar.gz
$ tar -zxvf helm-v3.0.0-linux-amd64.tar.gz
$ mv linux-amd64/helm /usr/local/bin/helm
```

## [Jenkins](https://www.jenkins.io/)
```shell
$ wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
$ sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
$ sudo apt update
$ sudo apt install jenkins
```

To start a jenkins server, run:
```shell
$ jenkins # it will start the jenkins it self
$ docker run -it -e NGROK_AUTHTOKEN=AUTH_TOKEN --net=host ngrok/ngrok http 8080
# this image will create a domain to the port in which jenkins is running (could be any service), http://localhost:8080/ -> https://some-domain. It is useful if you want to add a webhook that requires a web url, given that your localhost is not reachable from the web.
$ docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock
# this image will make a connection between jenkins and your local docker, so that you can use docker in jenkins pipelilne.
```

For more info about Jenkins and Docker, [read this](https://phoenixnap.com/kb/how-to-configure-docker-in-jenkins)

## Fonts
- MesloLGS NF (terminal)
- Fira Code (IDEs)
