# Zsh / Powerlevel10k

## Windows Only

윈도우: Git-Bash 설치  
https://github.com/git-for-windows/git/releases/download/v2.39.2.windows.1/Git-2.39.2-64-bit.exe

https://mirror.msys2.org/msys/x86_64/zsh-5.9-2-x86_64.pkg.tar.zst 파일 다운로드 및 압축 풀기  
압축 푼 파일에 있는 `etc`, `usr` 디렉토리를 `C:\Program Files\Git`에 복사

```bash
zsh
```

`~/.bashrc`
```bash
if [ -t 1 ]; then
    exec zsh
fi
```

## Common (Windows/MacOS)

### Oh-my-zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Powerlevel10k

Nerd 폰트 설치: https://github.com/romkatv/powerlevel10k#meslo-nerd-font-patched-for-powerlevel10k

터미널 폰트 변경

Powerlevel10k 설치

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

`~/.zshrc`

```bash
...
ZSH_THEME="powerlevel10k/powerlevel10k"
...
plugins=(git kubectl kube-ps1 kubectx helm)
...
```

터미널 재실행

powerlevel10k 구성

```bash
p10k configure
```
