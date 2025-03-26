# Transmission download speeds

<img width="376" alt="Transmission" src="https://github.com/FelixKratz/SketchyBar/assets/770120/b2fd02bd-c430-42c5-a956-12ee4e653118">

Shows upload and download speeds from Transmission torrent client. Written mostly by ChatGPT 😅 You need transmission-remote and Transmission app for this to work.

<details>
  <summary>sketchybarrc</summary>

```bash
source "$ITEM_DIR/transmission.sh"
```

</details>

<details>
  <summary>$ITEM_DIR/transmission.sh</summary>

```bash
#!/bin/env/bash

transmission=(
  drawing=off
  background.color=0xfff07178
  background.height=16
  background.corner_radius=16
  label.font="JetBrainsMono Nerd Font:Bold:11"
  label.color=0xbf181926
  label.padding_right=8
  icon.drawing=off
  update_freq=5
  updates=on
  script="$PLUGIN_DIR/transmission.sh"
  click_script="open -a /Applications/Transmission.app"
)

sketchybar --add item transmission right                \
           --set      transmission "${transmission[@]}"
```

</details>

<details>
  <summary>$PLUGIN_DIR/transmission.sh</summary>

```bash
#!/usr/bin/env bash

read UP DOWN <<< "$(transmission-remote -l | awk 'NR>1 {up=$4; down=$5} END {print up, down}')"
NUMBERS=($UP $DOWN)

# Number formatting. Thanks, ChatGPT!
for ((i=0; i<${#NUMBERS[@]}; i++)); do
    CURRENT_NUMBER=${NUMBERS[i]}

    # Check if the number is greater than 999
    if (( $(echo "$CURRENT_NUMBER > 999" | bc -l) )); then
        FORMATTED_NUMBER=$(echo "scale=1; $CURRENT_NUMBER / 1000" | bc -l)
        SUFFIX="MB"
    else
        FORMATTED_NUMBER=$(echo "scale=1; $CURRENT_NUMBER" | bc -l)
        SUFFIX="KB"
    fi

    # Remove the decimal point if it's zero
    if [[ $FORMATTED_NUMBER == *".0" ]]; then
        FORMATTED_NUMBER=${FORMATTED_NUMBER%".0"}
    fi

    # Create a new variable dynamically
    NEW_VARIABLE="FORMATTED_NUMBER_$i"
    declare "$NEW_VARIABLE=$FORMATTED_NUMBER$SUFFIX"
done

if [[ "$UP" == "0.0" && "$DOWN" == "0.0" ]]; then
  args+=(background.color=0x40eeeeee)
else
  args+=(background.color=0xfff78c6c)
fi


if [[ $NUMBERS != "" ]]; then
  args+=(drawing=on label="􀄯${FORMATTED_NUMBER_0} 􀄱${FORMATTED_NUMBER_1}")
else
  args=(drawing=off)
fi

sketchybar --set $NAME "${args[@]}"
```

</details>

EDIT: fixed plugin script.

---

Created by [@Pe8er](https://github.com/Pe8er)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-8107907)
