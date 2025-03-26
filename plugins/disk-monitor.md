
# Disk Capacity Monitor

Displays your main volume usage. Toggles label on click.

---

## sketchybarrc
```sh
sketchybar --add item diskmonitor right \
           --set diskmonitor icon.font="CaskaydiaCove Nerd Font Mono:Regular:24" \
                              icon.padding_right=20 \
                              label.drawing=off \
                              update_freq=360 \
                              script="$HOME/.config/sketchybar/plugins/diskmonitor.sh" \
           --subscribe diskmonitor mouse.clicked
```
diskmonitor.sh
```sh
#!/bin/bash
PERCENTAGE=$(df -H /System/Volumes/Data | awk 'END {print $5}' | sed 's/%//')

# possibly switch icons or colors based on usage
# toggles label drawing on click
```
