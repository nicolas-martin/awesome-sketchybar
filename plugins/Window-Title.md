# Window Title

__DESCRIPTION__: This will display the current window's title in your bar. If the window's title is greater than 50 characters it will truncate it, if there it no title available it will fall back to the application name. This was written by @FelixKratz and Modified by me.

__NOTE__: Assuming you are using yabai, you can quickly check window titles and app names of the current window with `yabai -m query --windows --window | jq '.app,.title'`.

Updated Configuration:

<details>
   <summary>sketchybarrc</summary>

```SHELL
# E V E N T S
sketchybar -m --add event window_focus \
              --add event title_change

# W I N D O W  T I T L E 
sketchybar -m --add item title left \
              --set title script="$HOME/.config/sketchybar/plugins/window_title.sh" \
              --subscribe title window_focus front_app_switched space_change title_change
```
</details>

<details>
   <summary>window_title.sh</summary>

```SHELL
#!/bin/bash

# W I N D O W  T I T L E 
WINDOW_TITLE=$(/opt/homebrew/bin/yabai -m query --windows --window | jq -r '.title')

if [[ $WINDOW_TITLE = "" ]]; then
  WINDOW_TITLE=$(/opt/homebrew/bin/yabai -m query --windows --window | jq -r '.app')
fi

if [[ ${#WINDOW_TITLE} -gt 50 ]]; then
  WINDOW_TITLE=$(echo "$WINDOW_TITLE" | cut -c 1-50)
  sketchybar -m --set title label="│ $WINDOW_TITLE"…
  exit 0
fi

sketchybar -m --set title label="│ $WINDOW_TITLE"
```
</details>

<details>
   <summary>yabairc</summary>

```SHELL
  # S K E T C H Y B A R  E V E N T S
    yabai -m signal --add event=window_focused action="sketchybar -m --trigger window_focus &> /dev/null"
    yabai -m signal --add event=window_title_changed action="sketchybar -m --trigger title_change &> /dev/null"
```
</details>
Felix: Update to V2.0.0 Syntax

---

Created by [@typkrft](https://github.com/typkrft)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1215932)
