# MPD

Displays MPD’s current track info and playback state. Toggles via mpc on click.

---

## sketchybarrc
```sh
sketchybar -m --add item mpd right \
              --set mpd update_freq=2 \
                        script="~/.config/sketchybar/plugins/mpd.sh" \
                        click_script="mpc toggle"

```
mpd.sh
```
#!/usr/bin/env bash

if [ "$(mpc status | wc -l | tr -d ' ')" == "1" ]; then
  ICON=""
  LABEL=""
else
  artist=$(mpc current -f %artist%)
  song=$(mpc current -f %title%)
  state=$(mpc status | awk 'NR==2 {print $1}') # e.g. [playing]

  if [ "$state" = "[playing]" ]; then
    ICON=""
  else
    ICON=""
  fi

  LABEL="${artist} • ${song}"
fi

sketchybar -m --set mpd icon="$ICON" label="$LABEL"
```
