# Mic Status

Toggles mic mute/unmute with a click. Icon changes accordingly.

---

## sketchybarrc
```sh
sketchybar -m --add item mic right \
              --set mic update_freq=3 \
                        script="~/.config/sketchybar/plugins/mic.sh" \
                        click_script="~/.config/sketchybar/plugins/mic_click.sh"
```

mic.sh
```sh
#!/bin/bash
MIC_VOLUME=$(osascript -e 'input volume of (get volume settings)')

if [[ $MIC_VOLUME -eq 0 ]]; then
  sketchybar -m --set mic icon=""
else
  sketchybar -m --set mic icon=""
fi
```
mic_click.sh
```
#!/bin/bash
MIC_VOLUME=$(osascript -e 'input volume of (get volume settings)')

if [[ $MIC_VOLUME -eq 0 ]]; then
  osascript -e 'set volume input volume 25'
  sketchybar -m --set mic icon=""
else
  osascript -e 'set volume input volume 0'
  sketchybar -m --set mic icon=""
fi
```
