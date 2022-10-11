Languages and input method supports
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

IME framework suggestion:
1. Use `fcitx5` if it supports you language in Debian Stable or flatpak, unless you are on gnome desktop
2. Use `ibus` if you are on gnome desktop
3. Use `ibus` or `fcitx` if your language is not supported well in `fcitx5` without using Testing/Sid packages
4. Do not use `uim` or `scim` because they are out of date. Few people still using them so there could be many unclear problems
