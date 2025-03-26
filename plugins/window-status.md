## window status

**Description**

item that shows the app name in bold, window name, and the properties of the window as set by yabai (floating, topmost, zoom_fullscreen, sticky)

Improved version of the [Window Title plugin](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1215932)

`yabai_command_run` is an event that should be called whenever a yabai command is run. This should be put in `skhdrc` if you use it, and as a terminal alias.

Something like this would work nicely:
```sh
yabai() {
    command yabai "$@"
    sketchybar -m --trigger yabai_command_run >/dev/null 2>/dev/null &disown # prevents any delay from happening
}
```

<img width="508" alt="image" src="https://user-images.githubusercontent.com/72744903/141528681-ad6b3f86-b96f-4e48-b634-c39542886759.png">

<details>
<summary>sketchybarrc</summary>

```sh
sketchybar -m \
    --add item app_name left \
    --set app_name \
        label.font="SF Mono:Heavy:13.0" \
        label.padding_right=0 \
        background.padding_left=2 \
        background.padding_right=0 \
    --add item window left \
    --set window \
        script="$PLUGIN_DIR/window.sh" \
        background.padding_left=0 \
        background.padding_right=0 \
    --subscribe window \
        application_front_switched \
        window_focused \
        window_title_changed \
        space_changed \
        yabai_command_run \
    --add bracket app_window \
        app_name window \
    --set app_window \
        background.drawing=on \
```

</details>

<details>
<summary>window.sh</summary>

```sh
#!/bin/zsh

# window title
data=$(yabai -m query --windows --window)

window_title=$(echo $data | jq -r '.title' \
    | sed 's/ - Eric (Person [[:digit:]])$//g' \
    | sed 's/ - Google Chrome$//g' \
    | sed 's/ - Audio playing$//g' \
    | sed 's/ - Camera or microphone recording$//g' \
    | sed 's/ - Part of group ..*$//g')

[ "${#window_title}" -gt 40 ] && window_title="$(echo $window_title | head -c 40)…"

# window properties

floating=$(echo $data | jq -r '.floating')
topmost=$(echo $data | jq -r '.topmost')
sticky=$(echo $data | jq -r '.sticky')
zoom_fullscreen=$(echo $data | jq -r '."zoom-fullscreen"')
zoom_parent=$(echo $data | jq -r '."zoom-parent"')

[ "$floating" = "1" ] && properties="${properties}~"
[ "$topmost" = "1" ] && properties="${properties}^"
[ "$zoom_fullscreen" = "1" ] && properties="${properties}f"
[ "$zoom_parent" = "1" ] && properties="${properties}p"
[ "$sticky" = "1" ] && properties="${properties}*"

if ! [ -z "$properties" ]; then
    [ -z "$window_title" ] && window_title="[$properties]" || window_title="$window_title [$properties]"
fi

# app name

app_name=$(echo $data | jq -r '.app')

[ "${#app_name}" -gt 20 ] && app_name="$(echo $app_name | head -c 20)…"

# setting items

sketchybar -m \
    --set app_name \
        label="$app_name" \
        drawing=$([ -z "$app_name" ] && echo off || echo on) \
    --set window \
        label="$window_title" \
        drawing=$([ -z "$window_title" ] && echo off || echo on)
```

</details>


---

Created by [@es183923](https://github.com/es183923)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1633997)
