---

## /plugins/window-title.md

```markdown
# Window Title

Displays the current window’s title. Falls back to the app name if no title is present, truncates if too long.

---

## sketchybarrc
```sh
# E V E N T S
sketchybar -m --add event window_focus
sketchybar -m --add event title_change

# Add the 'title' item
sketchybar -m --add item title left \
              --set title script="$HOME/.config/sketchybar/plugins/window_title.sh" \
              --subscribe title window_focus front_app_switched space_change title_change

```
#!/bin/bash

WINDOW_TITLE=$(/opt/homebrew/bin/yabai -m query --windows --window | jq -r '.title')

if [[ -z "$WINDOW_TITLE" ]]; then
  WINDOW_TITLE=$(/opt/homebrew/bin/yabai -m query --windows --window | jq -r '.app')
fi

if [[ ${#WINDOW_TITLE} -gt 50 ]]; then
  WINDOW_TITLE="${WINDOW_TITLE:0:50}"
fi

sketchybar -m --set title label="│ $WINDOW_TITLE"

```
