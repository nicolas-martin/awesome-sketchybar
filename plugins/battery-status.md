# Battery Status

Displays battery percentage, charging status, and optional icons for different levels.

---

## sketchybarrc
```sh
sketchybar -m --add item battery right \
              --set battery update_freq=3 \
                            script="~/.config/sketchybar/plugins/power.sh" \
                            icon=""
```


```sh
#!/bin/bash

BATT_PERCENT=$(pmset -g batt | grep -Eo "\d+%" | cut -d% -f1)
CHARGING=$(pmset -g batt | grep 'AC Power')

if [[ $CHARGING != "" ]]; then
  sketchybar -m --set battery icon="" label="${BATT_PERCENT}%"
  exit 0
fi

# Additional logic for icons/colors based on BATT_PERCENT
```
