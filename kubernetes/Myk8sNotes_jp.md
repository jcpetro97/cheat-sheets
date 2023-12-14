# My personal [[k8s]] notes


## Install kubectl

### [[Linux]]

- Update the `apt` package index and install packages needed to use the Kubernetes `apt` repository

```shell
sudo apt-get update
sudo apt-get install -y ca-certificates curl
```

- Download the Google Cloud public signing key

```
curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg
```

- Add the Kubernetes `apt` repository

```shell
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

- Update `apt` package index with the new repository and install kubectl:

```shell
sudo apt-get update
sudo apt-get install -y kubectl
```

**Note:** In releases older than Debian 12 and Ubuntu 22.04, `/etc/apt/keyrings` does not exist by default. You can create this directory if you need to, making it world-readable but writeable only by admins.

### [[macos]]

```bash
brew install kubectl
```

**Note:** 


## Install krew

1. Make sure that `git` is installed.
    
2. Run this command to download and install `krew`:
    
    ```sh
    (
      set -x; cd "$(mktemp -d)" &&
      OS="$(uname | tr '[:upper:]' '[:lower:]')" &&
      ARCH="$(uname -m | sed -e 's/x86_64/amd64/' -e 's/\(arm\)\(64\)\?.*/\1\2/' -e 's/aarch64$/arm64/')" &&
      KREW="krew-${OS}_${ARCH}" &&
      curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/${KREW}.tar.gz" &&
      tar zxvf "${KREW}.tar.gz" &&
      ./"${KREW}" install krew
    )
    ```
    
3. Add the `$HOME/.krew/bin` directory to your PATH environment variable. To do this, update your `.bashrc` or `.zshrc` file and append the following line:
    
    ```sh
    export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"
    ```
    
    and restart your shell.
    
4. Run `kubectl krew` to check the installation.