# Front Application Name
Displays the currently selected application name without using any additional software (like yabai etc.).
This works because the `front_app_switched` event now sends the app name in the `$INFO` variable to the script.

<details>
   <summary>sketchybarrc</summary>

```bash
sketchybar --add item system.label left \
           --set system.label script="sketchybar --set \$NAME label=\"\$INFO\"" \
           --subscribe system.label front_app_switched
```
</details>


---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1955400)
