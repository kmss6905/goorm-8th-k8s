# Install Tools

## Windows

### Chocolatey

Powershell 관리자 권한으로 설치

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

### awscli

```bash
choco install awscli
```

### aws-iam-authenticator

```bash
choco install aws-iam-authenticator
```

### kubectl

```bash
choco install kubernetes-cli --version=1.24.10
```

### eksctl

```bash
choco install eksctl
```

### helm

```bash
choco install helm
```

## MacOS

### Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### awscli

```bash
brew install awscli
```

### aws-iam-authenticator

```bash
brew install aws-iam-authenticator
```

### kubectl

```bash
curl -LO "https://dl.k8s.io/release/v1.24.6/bin/darwin/arm64/kubectl"
```

```bash
sudo install kubectl /usr/local/bin
```

### helm

```bash
brew install helm
```
