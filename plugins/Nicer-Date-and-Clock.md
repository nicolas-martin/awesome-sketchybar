# Nicer Date and Clock
Just a very simple design for the clock
![Screen Shot 2021-09-24 at 01 09 54](https://user-images.githubusercontent.com/22680421/134596006-aa4335e3-3ad1-4755-95b5-68163e639ae3.jpg)

<details>
  <summary>sketchybarrc</summary>

```bash
sketchybar -m --add       item               time             right                                        \
              --set       time               update_freq=2                                                 \
                                             icon.padding_right=0                                          \
                                             label.padding_left=0                                          \
                                             script="~/.config/sketchybar/plugins/time.sh"                 \
                                                                                                           \
              --add       item               date             right                                        \
              --set       date               update_freq=60                                                \
                                             background.color=0xffe8e8e9                                   \
                                             label.color=0xff000000                                        \
                                             label.font="SF Pro:Semibold:12"                               \
                                             icon.padding_right=0                                          \
                                             label.padding_left=0                                          \
                                             background.height=15                                          \
                                             background.corner_radius=4                                    \
                                             script="~/.config/sketchybar/plugins/date.sh"
```
</details>

<details>
  <summary>date.sh</summary>

```bash
#!/usr/bin/env bash

sketchybar -m --set $NAME label="$(date '+%a %d. %b')"
```
</details>
<details>
  <summary>time.sh</summary>

```bash
#!/usr/bin/env bash

sketchybar -m --set $NAME label="$(date '+%H:%M')"
```
</details>

---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1377164)
