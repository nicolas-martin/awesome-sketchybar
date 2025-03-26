# SketchyVim Indicator
A small indicator for [SketchyVim](https://github.com/FelixKratz/SketchyVim).
Displays the mode and the commandline status.

https://user-images.githubusercontent.com/22680421/153713383-e55d648b-f670-48e4-b53c-d36ee401119a.mp4


<details>
   <summary>sketchybarrc</summary>

```bash
sketchybar --add item svim.mode right \
           --set svim.mode popup.align=right \
                           script="sketchybar --set svim.mode popup.drawing=off" \
           --subscribe svim.mode front_app_switched window_focus \
           --add item svim.cmdline popup.svim.mode \
           --set svim.cmdline icon="Command: "
```
</details> 

<details>
   <summary>svim.sh</summary>

```bash
#!/usr/bin/env sh

# This script is executed when either the mode changes,
# or the commandline changes

sketchybar --set svim.mode icon="[$MODE]" \
           --set svim.cmdline label="$CMDLINE"

if [ "$CMDLINE" != "" ]; then
  sketchybar --set svim.mode popup.drawing=on
else
  sketchybar --set svim.mode popup.drawing=off
fi
```
</details> 


---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-2163174)
