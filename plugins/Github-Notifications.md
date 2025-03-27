# Github Notifications
Shows github notifications in a popup with some nice color coding. Uses `jq` and `gh`. `gh` must be authenticated.

<img width="383" alt="Screen Shot 2022-01-23 at 02 19 44" src="https://user-images.githubusercontent.com/22680421/150660995-c29c10b5-11a1-4a25-bee5-55bbd9875c15.png">

The bell is only red if some notification is considered important.

Clicking the item will directly open the corresponding website.

<details>
   <summary>sketchybarrc</summary>

```bash
sketchybar  --add       item               github.bell left                         \
           --set       github.bell        associated_space=1                       \
                                          update_freq=180                          \
                                          icon.font="$FONT:Bold:15.0"              \
                                          icon=􀋙                                   \
                                          label=$LOADING                           \
                                          script="$PLUGIN_DIR/gitNotifications.sh" \
                                          click_script="sketchybar --set \$NAME popup.drawing=toggle"
```
</details> 

<details>
   <summary>gitNotifications.sh</summary>

```bash
#!/usr/bin/env sh

NOTIFICATIONS="$(gh api notifications)"
COUNT="$(echo "$NOTIFICATIONS" | jq 'length')"
args=()
if [ "$NOTIFICATIONS" = "[]" ]; then
  args+=(--set $NAME icon=􀋚 label="0")
else
  args+=(--set $NAME icon=􀝗 label="$COUNT")
fi

# For sound to play around with:
# afplay /System/Library/Sounds/Morse.aiff

args+=(--remove '/github.notification\.*/')

COUNT=0
COLOR=0xffe1e3e4
args+=(--set github.bell icon.color=$COLOR)

while read -r repo url type title 
do
  COUNT=$((COUNT + 1))
  IMPORTANT="$(echo "$title" | egrep -i "(deprecat|break|broke)")"
  COLOR=0xff72cce8
  PADDING=0
  case "${type}" in
    "'Issue'") COLOR=0xff9dd274; ICON=􀍷; PADDING=0; URL="$(gh api "$(echo "${url}" | sed -e "s/^'//" -e "s/'$//")" | jq .html_url)"
    ;;
    "'Discussion'") COLOR=0xffe1e3e4; ICON=􀒤; PADDING=0; URL="https://www.github.com/notifications"
    ;;
    "'PullRequest'") COLOR=0xffba9cf3; ICON="􀙡"; PADDING=4; URL="$(gh api "$(echo "${url}" | sed -e "s/^'//" -e "s/'$//")" | jq .html_url)"
    ;;
  esac
  
  if [ "$IMPORTANT" != "" ]; then
    COLOR=0xffff6578
    ICON=􀁞
    args+=(--set github.bell icon.color=$COLOR)
  fi
  args+=(--add item github.notification.$COUNT popup.github.bell                                      \
         --set github.notification.$COUNT background.padding_left=7                                   \
                                          background.padding_right=7                                  \
                                          background.color=0x22e1e3e4                                 \
                                          background.drawing=off                                      \
                                          icon.background.height=1                                    \
                                          icon.background.y_offset=-12                                \
                                          icon.background.color=$COLOR                                \
                                          icon.padding_left="$PADDING"                                \
                                          icon.color=$COLOR                                           \
                                          icon.background.shadow.color=0xff2a2f38                     \
                                          icon.background.shadow.angle=25                             \
                                          icon.background.shadow.distance=2                           \
                                          icon.background.shadow.drawing=on                           \
                                          icon="$ICON $(echo "$repo" | sed -e "s/^'//" -e "s/'$//"):" \
                                          label="$(echo "$title" | sed -e "s/^'//" -e "s/'$//")"      \
                                          script='case "$SENDER" in
                                                    "mouse.entered") sketchybar --set $NAME background.drawing=on
                                                    ;;
                                                    "mouse.exited") sketchybar --set $NAME background.drawing=off
                                                    ;;
                                                  esac' \
                                          click_script="open $URL;
                                                        sketchybar --set github.bell popup.drawing=off"
        --subscribe github.notification.$COUNT mouse.entered mouse.exited)
done <<< "$(echo "$NOTIFICATIONS" | jq -r '.[] | [.repository.name, .subject.latest_comment_url, .subject.type, .subject.title] | @sh')"

sketchybar -m "${args[@]}" 
```
</details> 

---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-2025720)
