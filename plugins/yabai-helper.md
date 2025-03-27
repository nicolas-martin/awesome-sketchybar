  # yabai helper

**DESCRIPTION**: shows the current yabai space, and the mode. Click the menu item to switch between the `bsp`, `stack`, and `float` modes.
**NOTES**: You can remove the space indicator if you use the space component.

<details>
   <summary>sketchybarrc</summary>

```sh
sketchybar -m --add item yabai right
sketchybar -m --set yabai update_freq=3
sketchybar -m --set yabai script="~/.config/sketchybar/plugins/yabai.sh"
sketchybar -m --subscribe yabai space_change
sketchybar -m --set yabai click_script="~/.config/sketchybar/plugins/yabai_click.sh"
```
</details>

<details>
   <summary>yabai.sh</summary>

```sh
#!/bin/bash

space_number=$(yabai -m query --spaces --display | jq 'map(select(."focused" == 1))[-1].index')
yabai_mode=$(yabai -m query --spaces --display | jq -r 'map(select(."focused" == 1))[-1].type')

sketchybar -m --set yabai label="$space_number:$yabai_mode"
```
</details>

<details>
   <summary>yabai_click.sh</summary>

```sh
#!/bin/bash

space_number=$(yabai -m query --spaces --display | jq 'map(select(."focused" == 1))[-1].index')
yabai_mode=$(yabai -m query --spaces --display | jq -r 'map(select(."focused" == 1))[-1].type')

case "$yabai_mode" in
    bsp)
    yabai -m config layout stack
    ;;
    stack)
    yabai -m config layout float
    ;;
    float)
    yabai -m config layout bsp
    ;;
esac

new_yabai_mode=$(yabai -m query --spaces --display | jq -r 'map(select(."focused" == 1))[-1].type')

sketchybar -m --set yabai label="$space_number:$new_yabai_mode"
```
</details>
Felix: Update to v2.0.0

---

Created by [@es183923](https://github.com/es183923)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1274836)
