# Volume Level

![Screen Shot 2022-02-16 at 7 59 05 PM](https://user-images.githubusercontent.com/5347925/154383853-cc65bd2d-b496-4e23-ba51-f55574033686.png)

Indicator showing system volume percentage, with an icon to indicate muted or unmuted state. 

<details>
  <summary>sketchybarrc</summary>

  ```bash
sketchybar                 \
  --add item sound right     \
  --set sound                \
  update_freq=5              \
  icon.color=0xffd08770      \
  script="$PLUGINS/sound.sh"
  ```
</details>

<details>
  <summary>sound.sh</summary>

  ```bash
#!/usr/bin/env bash

VOLUME=$(osascript -e "output volume of (get volume settings)")
MUTED=$(osascript -e "output muted of (get volume settings)")

if [[ $MUTED != "false" ]]; then
  ICON="ﱝ"
else
  case ${VOLUME} in
    100) ICON="";;
    9[0-9]) ICON="";;
    8[0-9]) ICON="";;
    7[0-9]) ICON="";;
    6[0-9]) ICON="";;
    5[0-9]) ICON="";;
    4[0-9]) ICON="";;
    3[0-9]) ICON="";;
    2[0-9]) ICON="";;
    1[0-9]) ICON="";;
    [0-9]) ICON="";;
    *) ICON=""
  esac
fi

sketchybar -m \
  --set $NAME icon=$ICON \
  --set $NAME label="$VOLUME%"
  ```
</details>

I will post any updates here as I make them.

---

Created by [@nbn22385](https://github.com/nbn22385)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-2192840)
