# Input Method Installation Guide
Input method (or input method editor, commonly abbreviated as IME or IM) is necessary for some Asian langauge like Chinese or Japanese. It can also used to input some symbols or ancient letters not used today anymore. For most multi languages users, even though swiching keyboard layout is enough, using IME to manage it can be a good choice.

In Linux world, there are several IMEs available. Users need to install basic IME framework and specific input methods (sometimes call as addons or engines) according to languages they use and also their habit. SpiralLinux does not pre-install IME due to such complicity, but we prefer a guide to help users to choose and install it. 

The specific condition of IM on Debian is kind of different from some other distros, so even those familiar with IM should better read this guide if you never use Debian before.

## Choice about Input Methods

### Languages and Input Method Supports
||fcitx5 from flatpak|fcitx5|fcitx5 from Sid|ibus|fcitx(4)|uim|scim|
|--|--|--|--|--|--|--|--|
|Simplifed Chinese|good|good|good|good|good|pinyin/wubi?|pinyin/wubi
|Traditional Chinese|good|Chinese.addons/chewing|good|good|good|pinyin/chewing|pinyin/wubi/chewing
|Japanese|mozc only|mozc/ssk|good|good|good|skk/mozc/anthy |skk/anthy|
|Korean|bad|yes|yes|yes|yes|yes|yes?|
|Vietnamese|yes|yes|yes|yes|yes|yes|yes
|Thai|bad|bad|yes|yes|yes|yes?|yes
|Other languages|bad|bad|yes|good|good|bad|bad
|rime| yes|yes|yes|yes|yes|no|no
|m17n| yes|no|yes|yes|yes|yes|yes
|table| no|partial|yes|yes|yes|no|yes
|----|
|wayland support|good|good|good|good for gnome, bad otherwise|no|no|no
|difficulty to install|easy|medium|high|medium|medium|medium|medium
|age|very new|new|very new|medium|old|out dated|out dated|


Note:
1. rime can be used to input most languages but need some config
2. m17n is a library to support many languages. It do support pinyin and hangul or so on, but it is a bad solution
3. table can be used to input many lanugages and symbols but not a good solution too
4. Chinese addons is a fcitx5 metapachage providing most Chinese input method excepting zhuyin/chewing
5. "bad" means it can be support by like rime\m17n\table but there is not any good out-of-box solution 

### IM Framework Suggestion:
1. Use `fcitx5` if it supports you language in Debian Stable or flatpak, unless you are on gnome desktop
2. Use `ibus` if you are on gnome desktop
3. Use `ibus` or `fcitx` if your language is not supported well in `fcitx5` without using Testing/Sid packages
4. Do not use `uim` or `scim` because they are out of date. Few people still using them so there could be many unclear problems
5. Using addon packages from Debian Tesing/Sid can be quite safe, but please not install basic `fcitx5` framework from there

## Ways to Install Input Method

### `fcitx5` from flatpak
#### IM Framework (Not using `im-config`)
`im-config` is Debian's program to choose IM framework to use. It can help you to auto starting IM when booting and help you to set environment variables. Though there are ways to avoid using it, this is not recommended.
First you need to add flathub repository to flatpak. It should have been added for SpiralLinux out-of-box.
```
sudo apt install fcitx5-frontend*
sudo flatpak install org.fcitx.Fcitx5 
sudo cp /var/lib/flatpak/exports/share/applications/fcitx5.desktop /etc/xdg/autostart
```
You may need to check auto start settings of desktop environment, depending on which destop environment you use.
Then adding such lines into `/etc/bash.bashrc` or `/etc/environment`(x86_64 PC for example):
```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export PATH=$PATH:/usr/lib/x86_64-linux-gnu/libgtk2.0-0:/usr/lib/x86_64-linux-gnu/libgtk-3-0
```
Then logout from session or just reboot. `fcitx5` should work now.
#### IM Framework (Using `im-config`)
