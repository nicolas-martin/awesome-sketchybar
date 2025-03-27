# Calendar popup!
Hi! I'm working on a calendar popup.
#### Here's a demo of my hard work!

https://github.com/FelixKratz/SketchyBar/assets/10196826/08463962-b0c5-413b-b52b-9a9a9a7b8e46

I am essentially trying to clone my waybar tooltip config for wayland. 

<details>
  <summary>plugins/sketchybarrc</summary>

```
datetime=(
  background.color=$BACKGROUND          # Set this color to your background color for popups!
  label.padding_left=15
  label.padding_right=15
  background.height=19 
  background.corner_radius=10
  popup.horizontal=on               # FIXME: FIGURE OUT HOW TO MAKE CALENDAR WITHOUT THIS!
  popup.align=center
  update_freq=60                    # required for datetime clock!
  script="$PLUGIN_DIR/datetime.sh"
  popup.height=135                  # REQUIRED for popup to work properly.
  background.corner_radius=10
)
#center items
sketchybar --add item datetime center \
  --set datetime "${datetime[@]}" \
  --subscribe datetime system_woke mouse.clicked mouse.entered mouse.exited mouse.exited.global # REQUIRED events for hover popup.
```
</details>

<details>
  <summary>plugins/date.sh</summary>

```
#!/bin/bash

# I like to source my colors, to reference them automatically in a colorScheme switch. 
source "$HOME/.config/sketchybar/colors.sh"

# Calendar output with gcal. MUST INSTALL gcal! requires awk and sed. 
CALENDAR=$(gcal | awk '{printf "%-21s\n", $0}' | sed -e 's|<|\[|g' -e 's|>|\]|g') 

LINE_1="| $(echo "$CALENDAR" | sed -n '2p') |"
LINE_2="| $(echo "$CALENDAR" | sed -n '3p') |"
LINE_3="| $(echo "$CALENDAR" | sed -n '4p') |"
LINE_4="| $(echo "$CALENDAR" | sed -n '5p') |"
LINE_5="| $(echo "$CALENDAR" | sed -n '6p') |"
LINE_6="| $(echo "$CALENDAR" | sed -n '7p') |"
LINE_7="| $(echo "$CALENDAR" | sed -n '8p') |"
LINE_8="| $(echo "$CALENDAR" | sed -n '9p') |"

declare -a lines=(
  "$LINE_1"
  "$LINE_2"
  "$LINE_3"
  "$LINE_4"
  "$LINE_5"
  "$LINE_6"
  "$LINE_7"
  "$LINE_8"
)

for (( i = 0; i < ${#lines[@]}; i++ )); do
  current_line="${lines[$i]}"
  if [[ -n "${lines[$i]}" && "${lines[$i]}" =~ [^[:space:]] ]]; then #FIXME: CURRENTLY NOT WORKING!
    row=(
      icon="$current_line"
      icon.padding_left=-3                    # set to -2 to hide behind border.
      icon.font="JetBrains Mono:Regular:12.0" # Non-negotiable!
      label.font="JetBrains Mono:Bold:12.0"   # Non-negotiable! 
      padding_left=0
      padding_right=0
      width=0
      y_offset=$(( 56 - 16 * i ))
      label="|"
      label.color=$GREY                       # Set this to your popup border color. Must be 2px at least!
      label.padding_left=-175 # To overwrite the '|' character on the left of the line. Fixes graphical text issues. 
      label.drawing=on
    )
    item_name="datetime.popup.cal_$((i+1))"
    sketchybar --add item "$item_name" popup.datetime --set "$item_name" "${row[@]}"
  fi
done

sketchybar --set "$item_name" background.padding_right=185

# Function to set date and time
function set_date_and_time {
  sketchybar --set $NAME label="$(date '+%a, %b %d  %I:%M %p')"
  sketchybar --set $NAME icon=$TIME
}

set_date_and_time # call it first

# Handle mouse events
case "$SENDER" in
  "mouse.entered")
    sketchybar --set $NAME popup.drawing=on
    #echo "Mouse Hovered in $NAME icon" >> /tmp/sketchybar_debug.log
    ;;
  "mouse.exited" | "mouse.exited.global")
    sketchybar --set $NAME popup.drawing=off
    #echo "Mouse left hover of $NAME icon" >> /tmp/sketchybar_debug.log
    ;;
  "mouse.clicked")
    sketchybar --set $NAME popup.drawing=toggle
    #echo "Mouse clicked on $NAME icon" >> /tmp/sketchybar_debug.log
    ;;
esac
```
</details>

### Caveats:
Requires gcal - `brew install gcal`
Requires JetBrains Mono - This is required! To provide proper spacing and overlay the '|' Character at the beginning of each lines. 
REPLACE the colors with your background popup color, and border color where the comments tell you to. 

### Todo: 
- [ ] Add left and right buttons to preview previous/upcoming months.
- [ ] Add option to view all months in the year
- [x] Add Trim to fix uneven whitespace on the right using AWK command piping
- [x] ~make the current day bold~ (Chose to use gcal < and > and replace with "[   ]" border instead)
- [x] ~Render without "|" Character at the beginning of each line~ (It still does this, made it hard to see)
- [x] rewrite code as an array and for loop instead of individually defining each variable as a line (Thanks @i-ilak!)
- [x] update cal less frequently than 1 second (Just update every minute instead. Could be changed!)
- [x] ~Multiline check if output empty~ (Not sure how to implement)


---

Created by [@aspauldingcode](https://github.com/aspauldingcode)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-7785678)
