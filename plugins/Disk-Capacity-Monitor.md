# Disk Capacity Monitor

<img width="340" alt="Untitled" src="https://github.com/FelixKratz/SketchyBar/assets/770120/886effc6-df82-4aad-af41-9c20c7363d77">

Shows usage of main system volume. Click on the icon to toggle label visibility.

You need to have [font-caskaydia-cove-nerd-font](https://github.com/ryanoasis/nerd-fonts) installed:

```bash
brew tap homebrew/cask-fonts
brew install font-caskaydia-cove-nerd-font
```

<details>
  <summary>sketchybarrc</summary>

```bash
source "$ITEM_DIR/diskmonitor.sh"
```

</details>

<details>
  <summary>$ITEM_DIR/diskmonitor.sh</summary>

```bash
#!/bin/bash

diskmonitor=(
  icon.font="CaskaydiaCove Nerd Font Mono:Regular:24"
  icon.padding_right=20
  label.drawing=off
  y_offset=1
  update_freq=360
  updates=when_shown
  script="$PLUGIN_DIR/diskmonitor.sh"
)

misc=(
  icon.drawing=off
  width=0
  padding_right=4
  update_freq=360
  updates=when_shown
)

diskmonitor_label=(
  "${misc[@]}"
  label.font="$FONT:Semibold:8"
  label=SSD
  y_offset=5
)

diskmonitor_value=(
  "${misc[@]}"
  label.font="$FONT:Bold:10"
  y_offset=-3
)

sketchybar                                          \
  --add item diskmonitor.label right                \
  --set diskmonitor.label "${diskmonitor_label[@]}" \
                                                    \
  --add item diskmonitor.value right                \
  --set diskmonitor.value "${diskmonitor_value[@]}" \
                                                    \
  --add item diskmonitor right                      \
  --set diskmonitor "${diskmonitor[@]}"             \
  --subscribe diskmonitor mouse.clicked
```

</details>

<details>
  <summary>$PLUGIN_DIR/diskmonitor.sh</summary>

```bash
#!/bin/bash

source "$CONFIG_DIR/colors.sh"

update() {
PERCENTAGE=$(df -H /System/Volumes/Data | awk 'END {print $5}' | sed 's/%//')

COLOR=$LABEL_COLOR

case ${PERCENTAGE} in
  9[8-9] | 100)
    ICON="󰪥"
    COLOR=$RED
    ;;
  8[8-9] | 9[0-7])
    ICON="󰪤"
    COLOR=$ORANGE
    ;;
  7[6-9] | 8[0-7])
    ICON="󰪣"
    COLOR=$YELLOW
    ;;
  6[4-9] | 7[0-5])
    ICON="󰪢"
    ;;
  5[2-9] | 6[0-3])
    ICON="󰪡"
    ;;
  4[0-9] | 5[0-1])
    ICON="󰪠"
    ;;
  2[8-9] | 3[0-9])
    ICON="󰪟"
    ;;
  1[6-9] | 2[0-7])
    ICON="󰪞"
    ;;
  [0-9] | 1[0-5])
    ICON="󰝦"
    ;;
  *)
    # Handle other cases if needed
    ICON="󰅚"
    COLOR=$HIGHLIGHT
    ;;
esac

sketchybar --set $NAME icon=$ICON icon.color=$COLOR
sketchybar --set $NAME.value label="$PERCENTAGE%"
}

label_toggle() {

  DRAWING_STATE=$(sketchybar --query $NAME.value | jq -r '.label.drawing')
  
  if [[ $DRAWING_STATE == "on" ]]; then
    DRAWING="off"
    PADDING="0"
  else
    DRAWING="on"
    PADDING="20"
  fi

  sketchybar --set $NAME.value label.drawing=$DRAWING \
             --set $NAME.label label.drawing=$DRAWING \
             --set $NAME icon.padding_right=$PADDING
}


case "$SENDER" in
"routine" | "forced")
  update
  ;;
"mouse.clicked")
  label_toggle
  ;;
esac
```
</details>

<details>
  <summary>$CONFIG_DIR/colors.sh</summary>

```bash
#!/bin/bash

# https://material-theme.com/docs/reference/color-palette/

# Material Ocean
# background": "#0F111A",
# "grey": "#3B4252",
# "cyan": "#89ddff",
# "blue": "#82aaff",
# "foreground": "#ffffff",
# "green": "#c3e88d",
# "red": "#ff5370",
# "yellow": "#ffcb6b"

O100=0xff
O75=0xbf
O50=0x80
O25=0x40
O10=0x1a

# Base Colors
export BASE_BLACK="181926" #0F111A
export BASE_WHITE="eeeeee" #eeeeee

# Color Palette
export BLACK=$O100$BASE_BLACK
export BLACK_75=$O75$BASE_BLACK
export BLACK_50=$O50$BASE_BLACK
export BLACK_25=$O25$BASE_BLACK
export WHITE=$O100$BASE_WHITE
export WHITE_75=$O75$BASE_WHITE
export WHITE_50=$O50$BASE_WHITE
export WHITE_25=$O25$BASE_WHITE
export WHITE_10=$O10$BASE_WHITE
export RED=0xffff5370 #ff5370
export GREEN=0xffc3e88d #c3e88d
export TEAL=0xff64FFDA #64FFDA
export CYAN=0xff89ddff #89ddff
export BLUE=0xff82aaff #82aaff
export OSBLUE=0xff0259D1 #0259D1
export YELLOW=0xffffcb6b #ffcb6b
export ORANGE=0xfff78c6c #f78c6c
export MAGENTA=0xffab47bc #ab47bc
export DARK_GREY=0xff292d3e #292d3e
export GREY=0xff676e95 #676e95
export GREY_50=0x80676e95 #676e95
export LIGHT_GREY=0xffa6accd #a6accd
export TRANSPARENT=0x00000000

# General bar colors
export BAR_COLOR=$BLACK_50
export CONTRAST=0xff34324a
export BAR_BORDER_COLOR=0xff2b2a3e
export ICON_COLOR=$WHITE
export ICON_COLOR_INACTIVE=$GREY
export LABEL_COLOR=$WHITE_75
export HIGHLIGHT=$TEAL
export POPUP_BACKGROUND_COLOR=$DARK_GREY
export POPUP_BORDER_COLOR=$TRANSPARENT
export SHADOW_COLOR=$BLACK
```
</details>

---

Created by [@Pe8er](https://github.com/Pe8er)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-8363347)
