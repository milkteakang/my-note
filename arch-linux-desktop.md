
##### virtualbox guest additions 대신 설치

```bash
pacman -S linux-headers
pacman -Syu virtualbox-guest-utils
systemctl enable vboxservice
```

##### 자동 로그인 설정

계정이 ubuntu라고 가정하고

```bash
sudo groupadd autologin
sudo gpasswd -a ubuntu autologin

vim /etc/lightdm/lightdm.conf
[Seat:*]
pam-autologin-service=lightdm-autologin
autologin-user=ubuntu
autologin-session=xfce
```

##### 한글 입력기 설치

```bash
pacman -S fcitx5-im fcitx5-hangul fcitx5-configtool
vi ~/.xprofile
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export INPUT_METHOD=fcitx
export SDL_IM_MODULE=fcitx
```

##### 기본 bash 설정

```bash
#
# ~/.bashrc
#

# If not running interactively, don't do anything
[[ $- != *i* ]] && return

if [ -x /usr/bin/dircolors ]; then
    # Use the default database
    eval "$(dircolors -b)"
fi

alias ls='ls --color=auto'
alias ll='ls -alF --color=auto'
alias la='ls -A'
alias l='ls -CF'
alias grep='grep --color=auto'
PS1='[\u@\h \W]\$ '
alias vi='vim'
export TIME_STYLE=long-iso
```

##### 기타 개발도구 설치

```bash
sudo pacman -S jdk-openjdk dbeaver
yay -S visual-studio-code-bin sublime-text-4 filezilla google-chrome
```
