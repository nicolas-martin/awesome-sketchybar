# Mic Status

__DESCRIPTION__: Check and Mute/Unmute your mic.

<details>
   <summary>sketchybarrc</summary>

```SHELL
sketchybar -m --add item mic right \
sketchybar -m --set mic update_freq=3 \
              --set mic script="~/.config/sketchybar/plugins/mic.sh" \
              --set mic click_script="~/.config/sketchybar/plugins/mic_click.sh"
```
</details>

<details>
   <summary>mic.sh</summary>

```SHELL
#!/bin/bash

MIC_VOLUME=$(osascript -e 'input volume of (get volume settings)')

if [[ $MIC_VOLUME -eq 0 ]]; then
  sketchybar -m --set mic icon=
elif [[ $MIC_VOLUME -gt 0 ]]; then
  sketchybar -m --set mic icon=
fi 
  
```
</details>

<details>
   <summary>mic_click.sh</summary>

```SHELL
#!/bin/bash

MIC_VOLUME=$(osascript -e 'input volume of (get volume settings)')

if [[ $MIC_VOLUME -eq 0 ]]; then
  osascript -e 'set volume input volume 25'
  sketchybar -m --set mic icon=
elif [[ $MIC_VOLUME -gt 0 ]]; then
  osascript -e 'set volume input volume 0'
  sketchybar -m --set mic icon=
fi
```
</details>
Felix: Update to v2.0.0

---

Created by [@typkrft](https://github.com/typkrft)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1216899)
