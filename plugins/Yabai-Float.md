## Yabai Float ##

#### Description ####

Used [es183923’s](https://github.com/es183923) Yabai Helper plugin as a jumping off point and made a plugin to display managed or floating state of individual windows with icon support! Clicking on the plugin will toggle managed state for the selected window.

<img width="68" alt="Screen Shot 2021-10-27 at T20 11 51" src="https://user-images.githubusercontent.com/53032923/139169247-a81f580e-814f-4e9a-9c7c-28d8b346d5e8.png">

Note: This plugin is the icon on the right. It is displaying that the current window is in floating mode. The icon on the left is my modified version of [es183923’s](https://github.com/es183923) Yabai Helper plugin which can be found [here](https://github.com/FelixKratz/SketchyBar/discussions/12?sort=old#discussioncomment-1549400).

<details><summary>sketchybarrc</summary><p>

```
sketchybar -m --add item yabai_float left \
              --set yabai_float update_freq=3 \
              --set yabai_float script="~/.config/sketchybar/plugins/yabai_float.sh" \
              --set yabai_float click_script="~/.config/sketchybar/plugins/yabai_float_click.sh" \
              --subscribe yabai_float space_change
```

</p></details>

<details><summary>yabai_float.sh</summary><p>

```
yabai_float=$(yabai -m query --windows --window | jq .floating)

case "$yabai_float" in
    0)
    sketchybar -m --set yabai_float label=""
    ;;
    1)
    sketchybar -m --set yabai_float label=""
    ;;
esac
```

</p></details>

<details><summary>yabai_float_click.sh</summary><p>

```
yabai -m window --toggle float

yabai_float=$(yabai -m query --windows --window | jq .floating)

case "$yabai_float" in
    0)
    sketchybar -m --set yabai_float label=""
    ;;
    1)
    sketchybar -m --set yabai_float label=""
    ;;
esac
```

</p></details>

---

Created by [@SxC97](https://github.com/SxC97)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1549412)
