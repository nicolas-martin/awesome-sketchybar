# Stack Indicator

Shows your yabai window’s stack position, either numeric or dots.

---

## sketchybarrc
```sh
sketchybar -m --add item stack_sep center \
           --add item stack center \
           --set stack script="$HOME/.config/sketchybar/plugins/stack.sh" \
                      update_freq=0 \
                      --subscribe stack window_focus front_app_switched space_change title_change \
           --set stack_sep drawing=off \
           --set stack drawing=off
```
stack.sh
```sh
#!/bin/bash

CURRENT=$(yabai -m query --windows --window | jq '.["stack-index"]')
if [[ $CURRENT -eq 0 ]]; then
  sketchybar -m --set stack label="" drawing=off
  exit 0
fi

LAST=$(yabai -m query --windows --window stack.last | jq '.["stack-index"]')

if [[ $LAST -gt 10 ]]; then
  sketchybar -m --set stack label="[${CURRENT}/${LAST}]" drawing=on
else
  dots=""
  for ((i=1; i<=$LAST; i++)); do
    if [[ $i -eq $CURRENT ]]; then
      dots+="●"
    else
      dots+="○"
    fi
  done
  sketchybar -m --set stack label="$dots" drawing=on
fi
```
