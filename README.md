# Localization
If you use non-English languages, you need to localize your system. Normally, localization needs two things: `locale` support and fonts, but sometimes input method is also needed. If you are newcomer and do not want to set all of these three ( or two ) byy step, you can just read `For Newcomers` chapter.

For locale suppports, SpiralLinux provides a program to install set, which you can easily find it out-of-box. If there are any problems, you can also install `locales-all` which provides all locale supoorts, and then edit environment variable to change languages. For example, you can edit `/etc/bash.bashrc` adding such line:
```
export LANG=zh_CN.utf8
```
This will provide Simplified Chinese as default language for you after reboot . You can see the 'code' of your language from `locale -a` after installing `locales-all`.

Besides, you need to install fonts so that system can show your language letters or characters, or it will show some strange squares usually called tofu. For most languages, SpiralLinux provides font support out of box. If not, you can install all `fonts-noto*` packages. 

If you used some Asian Languages, or you are multi-langauge user, you may want a input method. Input method (or input method editor, commonly abbreviated as IME or IM) is necessary for some Asian langauge like Chinese or Japanese. It can also used to input some symbols or ancient letters not used today anymore. For most multi languages users, even though swiching keyboard layout is enough, using IME to manage it can be a good choice.

However,installing IM can be kind of complex. For users want to choose and install IM themselves, please read the guidance below. Otherwise, following `For Newcomer` chapter may be a good choide for you.

## For Newcomers
If you want to set localiztion ( especially to instal IM ) as easily as possible, do not care about what IM you use, and especially you do not use multi languages, you can simply install task packages for localization. Task packages are like package groups of Fedora or patterns of openSUSE. You can install `tasksel` to set and install task packages, but use `apt` to directly install it is also good.

To search task packages, try `apt search task-`, most task packages here is for fully localization. For example,if you want Japanese, you can use:
```
apt install --install-recommends task-japanese task-japanese-desktop im-config
```
If you use gnome, you can also install `task-japanese-gnome-desktop`; If you use kde, you can also install `task-japanese-kde-dekstop`.

For other languages users, just change 'japanese' for your own language.  `im-config` is not necessary for those do not use asian languages. Otherwise, you can use `im-config` to choose IM for you.

# Input Method Guidance

In Linux world, there are several IMEs available. Users need to install basic IME framework and specific input methods (sometimes call as addons or engines) according to languages they use and also their habit. SpiralLinux does not pre-install IME due to such complicity, but we prefer a guide to help users to choose and install it. 

The specific condition of IM on Debian is kind of different from some other distros, so even those familiar with IM should better read this guide if you never use Debian before.

## Choice about Input Methods

### Languages and Input Method Supports
||fcitx5 from flatpak|fcitx5|fcitx5 from Sid|ibus|fcitx(4)|uim|scim|
|--|--|--|--|--|--|--|--|
|Simplifed Chinese|good|good|good|good|good|pinyin/wubi?|pinyin/wubi
|Traditional Chinese|good|chinese-addons/chewing|good|good|good|pinyin/chewing|pinyin/wubi/chewing
|Japanese|mozc only|mozc/ssk|good|good|good|skk/mozc/anthy |skk/anthy|
|Korean|bad|yes|yes|yes|yes|yes|yes?|
|Vietnamese|yes|bad|yes|yes|yes|yes|yes
|Thai|bad|bad|yes|yes|yes|yes?|yes
|Other languages|bad|bad|yes|good|good|bad|bad
|rime| yes|yes|yes|yes|yes|no|no
|m17n| yes|no|yes|yes|yes|yes|yes
|more table| no|no|yes|yes|yes|no|yes
|----|
|wayland support|good|good|good|good for gnome, bad otherwise|no|no|no
|difficulty to install|easy|easy|hard|medium|medium|medium|medium
|age|very new|new|very new|medium|old|out dated|out dated|


Note:
1. `rime` can be used to input most languages but need some config (see `Furthermore & Toubleshooting` )
2. `m17n` is a library to support many languages, but usually it is a bad solution
3. `table` can be used to input many lanugages and symbols but not a good solution too
4. `chinese-addons` is a fcitx5 metapachage providing most Chinese input method excepting zhuyin/chewing
5. "bad" means it can be support by like rime\m17n\table but there is not any good out-of-box solution 
6. `fcitx` sometimes also called 'fcitx4' to be distinguished from `fcitx5`. They are maintained by one developer.

### IM Framework Suggestion:
1. Use `fcitx5` if it supports you language in Debian Stable or flatpak, unless you are on gnome desktop
2. Use `ibus` if you are on gnome desktop
3. Use `ibus` or `fcitx` if your language is not supported well in `fcitx5` without using Testing/Sid packages
4. Do not use `uim` or `scim` because they are out of date. Few people still using them so there could be many unclear problems
5. Using addon packages from Debian Tesing/Sid can be quite safe, but please not install basic `fcitx5` framework from there

## Ways to Install Input Method

### `fcitx5` from flatpak
#### IM Framework (Not using `im-config`)
`im-config` is Debian's program to choose IM framework to use. It can help you to auto starting IM when booting and help you to set environment variables. However, if you choose to install `fcitx5` from flatpak, `im-config` cannot work for it, so we have to make a hack.
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
#### ~~IM Framework (using `im-config`)~~ Need a patch or some config
#### IM Engines
##### For Simplified Chinese
```
sudo flatpak install org.fcitx.Fcitx5.Addon.ChineseAddons
```
##### For Traditional Chinese
```
sudo flatpak install org.fcitx.Fcitx5.Addon.ChineseAddons
```
Then if you want chewing/zhuyin
```
sudo flatpak install org.fcitx.Fcitx5.Addon.Zhuyin
```
or
```
sudo flatpak install org.fcitx.Fcitx5.Addon.Chewing
```
`zhuyin` uses `libpinyin` project. You can choose one as you like.
##### For Japanese
Only Mozc available. `fcitx5` in flatpak is not a good choice for Japanese users now.
```
sudo flatpak install org.fcitx.Fcitx5.Addon.Mozc
```
##### For Vietnamese
```
sudo flatpak install org.fcitx.Fcitx5.Addon.Unikey
```
##### If you want M17N 
```
sudo flatpak install org.fcitx.Fcitx5.Addon.M17N
```
##### If you want rime 
```
sudo flatpak install org.fcitx.Fcitx5.Addon.Rime
```
### `fcitx5` from Debian Stable
#### IM Framework
```
sudo apt install --install-recommends fcitx5 im-config
```
Here `--install-recommends` is necessary, or `fcitx5` on Debian will not work. The major point this Debian makes `fcitx5-module-xorg` as a individual package, which is differrnt from many distros. Do not forget it.
Then use non-root user to run
```
im-config
```
You can choose `fcitx5` here. Then logout from session or just reboot. `fcitx5` should work now.
##### For kde users
You can install `kde-config-fcitx5` but it is not necessary.
You can use kimpanel. It will make IM look much better, but conflict with theme package `fcitx5-material-color`.
##### For gnome users
Please install `gnome-shell-extension-kimpanel` too. This is necessary if you want yo to use `fcitx5` on gnome.
##### For other desktops
If you want some themes, you can install `fcitx5-material-color`.
#### IM Engines
Some addons are absent here. That is because `fcitx5` is a quite young program so that many of its addons have not been included into Debian stable. You can using packages from Testing or Sid. See `Furthermore & Toubleshooting`.
##### For Simplified Chinese
```
sudo apt install --install-recommends fcitx5-chinese-addons
```
Here `--install-recommends` may not be necessary but highly recommended.
##### For Traditional Chinese
```
sudo apt install --install-recommends fcitx5-chinese-addons
```
Then if you want chewing
```
sudo apt install --install-recommends fcitx5-chewing
```
`zhuyin` using `libpinyin` is absent here.
##### For Japanese
You can only install one of there two. However, `kkc` and  `anthy` are absent here.
```
sudo apt install --install-recommends fcitx5-mozc fcitx5-ssk
```
##### For Korean
```
sudo apt install --install-recommends fcitx5-hangul
```
##### If you want rime 
```
sudo apt install --install-recommends fcitx5-rime
```

### `ibus`
`ibus` is probably the most widely used IME in Linux world due to it is highly integrated into gnome desktop. Using `ibus` on gnome desktop can easily gain a good experience. However, on other desktops it is widely thought that `fcixt5` is better.  One strong reason to use `ibus` in SpiralLinux is that in Debian 11 it supports much more languages officially. However, `ibus` does not support wayland well on non-gnome desktop. 

#### IM framwwork
```
sudo apt install --install-recommends ibus im-config
```
Then use `im-config` to set IM framwork and logout or reboot to make it work.

#### IM Engines
`ibus` provides most language support in Debian Stable.
##### For Chinese
If you need pinyin IM:
```
sudo apt install --install-recommends ibus-linpyin
```
If you need zhuyin IM, you can choose `ibus-libzhuyin` or `ibus-chewing`. Please do not use`ibus-pinyin` and `ibus-zhuyin` because they are aged and not maintained anymore.

##### For Japanese
`ibus-kkc` `ibus-ssk` `ibus-anthy` and `ibus-mozc` are all available here. Just choose what you want.

##### For Korean
```
sudo apt install --install-recommends ibus-hangul
```
##### For Vietnamese
```
sudo apt install --install-recommends ibus-unikey
```
##### For Thai
```
sudo apt install --install-recommends ibus-libthai
```
##### If you want rime 
```
sudo apt install --install-recommends ibus-rime
```
##### If you want m17n
```
sudo apt install --install-recommends ibus-m17n
```
##### If you want more table or other language support
Please search for `ibus-table*` packages for extra tables, and `ibus-*` packages for more IM. If here is no language support of what you want, please think about using `fcitx`.

### `fcitx`
`fcitx`, sometimes also called fcitx4 to be distinguished from `fcitx5`, is older version that still be maintained now. It does not support wayland. The only reason to use it rather than `fcitx5` maybe is it support more addons, especially on Debian 11. It can be the best choice if you need to use languages like Sinhala. 

#### IM framework
```
sudo apt install --install-recommends fcitx im-config
```
Then use `im-config` to set IM framwork and logout or reboot to make it work.

If you need themes, you can install `fcitx-ui*` packages.
#### IM Engines
The package name of `fcitx` is similar to it of `fcitx5`. You can search them for yourself. If your lanaguages are not supported, please think about searching your languages under `ibus`.

## Furthermore & Troubleshooting
### What is `rime`
```
"Rime is an input method engine for entering Chinese characters, supporting a wide range of input methods. The Rime engine itself does not provide a frontend for receiving user input. It must be used with an input method framework such as `fcitx5` or `ibus`. " 
——ArchWiki
```
`rime` is a IM engine/addon that can be used on various IM framework. As a result, you can use `rime` on different platforms and operating systems like Windows, Macos and Linux, so that you can get similar input experience on different OS. 

One major difference between `rime` and other IM engine is that it does not limit input schemas. Users can, and actually need, to add input schemas to make it work, although mostly `rime-luna-pinyin` is used as default. 

Thanks to such design, `rime` can not only provide Chinses input method but also have potential to be a general IM engine. Using `rime`, you can type any symbols,emoji or other languages if such schemas has been made by any developer. For example, you can make rime-based Greek input schemas on `ibus-rime` to avoid a known bug of `ibus`if you use Chinese and Greek on `ibus` at the same time.

However, to realize this potential, you need to spend much time config `rime`, especially when you have to make you own schemas. So even though `rime` provides IM for every langauge in theory, it may not be as powerful in practice.

### `m17n` and `table`

`m17n` is a Gnu project to support many languages. However, many IM schemas in `m17n` is not good for using. Many packages named as `xx-m17n` even do not provide all the supported languages of `m17n`, so that it should not be seen as solution when you have other choices.

`table` actually means "code table". For primary understanding, it can be seen as library of IM. However, for some IM framework, table packages also include some IM engine. For example, `fcitx-table-extra` provides `cangjie` which can help to input some strange characters. However, mostly these packages are just for "strange and rare symbols" which are not common used at all, so you should not look upon them as IM solution.

### `fcitx5` packages from Sid
Debian has officical backports repository. However, IM packages are not included. As a result, if you want to use newer `fcitx5` packages, you need to install them from Debian Testing or Sid repository.

Please do not add repository directly for `apt` if you do not want to update your whole system to Testing or Sid, but you can download their `.deb` packages. Besides, installing IM framework packages is no safe. You may encounter with many problem, especially due to different version of `gtk` and `qt`. On the other hand, using addon packages from Sid is much safer, although there is no promised that everything must be OK.

### Can not activate input method in `ibus`?
If you have non-English alphabet languages like Greek or Russian in `ibus`, at the same time you use language like Chinese, you will probably find you cannot activate Chinese input method engine from Greek or Russian. You mmay have to switch to English first then you can switch to Chinese IM to activate it. This known bug is intended to do so, becuase otherwise you may be not able to use Chinese IM with alernative keyboard layout. You have to live with it if you choose to use `ibus`, unless you decide to use `rime` to input all languages.
