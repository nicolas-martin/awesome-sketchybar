# Battery Status

__DESCRIPTION__: This will display your current battery status.

__NOTE__: This could probably be improved on greatly. I am not sure what an appropriate `update_freq` is for this so feel free to adjust it to your liking. 

__UPDATED__: Updated to implement improvements made by [ut0mt8](https://github.com/ut0mt8) from [this](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1357508) comment.

<details>
   <summary>sketchybarrc</summary>

```BASH
sketchybar -m --add item battery right \
              --set battery update_freq=3 \
              --set battery script="~/.config/sketchybar/plugins/power.sh" \
              --set battery icon=
```
</details>

<details>
   <summary>power.sh</summary>

```BASH
#!/bin/bash

. ~/.cache/wal/colors.sh
BATT_PERCENT=$(pmset -g batt | grep -Eo "\d+%" | cut -d% -f1)
CHARGING=$(pmset -g batt | grep 'AC Power')

if [[ $CHARGING != "" ]]; then
  sketchybar -m --set battery \
    icon.color=0xFF5DFE67 \
    icon= \
    label=$(printf "${BATT_PERCENT}%%")
  exit 0
fi

[[ ${BATT_PERCENT} -gt 10 ]] && COLOR=0xFF${color5:1} || COLOR=0xFFFF0000

case ${BATT_PERCENT} in
   100) ICON="" ;;
    9[0-9]) ICON="" ;;
    8[0-9]) ICON="" ;;
    7[0-9]) ICON="" ;;
    6[0-9]) ICON="" ;;
    5[0-9]) ICON="" ;;
    4[0-9]) ICON="" ;;
    3[0-9]) ICON="" ;;
    2[0-9]) ICON="" ;;
    1[0-9]) ICON="" ;;
    *) ICON=""
esac

sketchybar -m --set battery\
  icon.color=$COLOR \
  icon=$ICON \
  label=$(printf "${BATT_PERCENT}%%")
```
</details>
Felix: Update to v2.0.0

---

Created by [@typkrft](https://github.com/typkrft)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1216590)
