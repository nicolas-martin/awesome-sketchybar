# Apple Calendar
<img width="219" alt="Screenshot 2023-01-18 at 10 26 32" src="https://user-images.githubusercontent.com/114060741/213144811-a4fb5948-73a6-4f25-9b5d-64a06bf72fc5.png">


**DESCRIPTION:** This plugin displays the time of the next upcoming event of the day, noted in your apple calendar, in your bar. When you hover over the icon, a popup appears with a list of all the events of the day.

**NOTE:** This plugin uses [icalBuddy](https://hasseg.org/icalBuddy/), so you will have to install this manually (with brew: `brew install ical-buddy`). There might be issues with the calendar access. If so, then try to execute this command in your terminal: `osascript -e 'tell application "Calendar" to calendars'` (for more info, read [this](https://c33tech.com/blog/2022/11/ventura__keyboard_maestro__icalbuddy__confusion/)).

<details>
    <summary>sketchybarrc</summary>
    
    #!/bin/sh
    POPUP_CLICK_SCRIPT="sketchybar --set ical popup.drawing=toggle"
    
    sketchybar --add       item            ical right                         \
               --set       ical            update_freq=180                    \
                                           icon=􀉉                             \
                                           popup.align=right                  \
                                           script="$PLUGIN_DIR/ical.sh"       \
                                           click_script="$POPUP_CLICK_SCRIPT" \
               --subscribe ical            mouse.entered                      \
                                           mouse.exited                       \
                                           mouse.exited.global                \
                                                                              \
               --add       item            ical.template popup.ical           \
               --set       ical.template   drawing=off                        \
                                           background.corner_radius=12        \
                                           padding_left=7                     \
                                           padding_right=7                    \
                                           icon.background.height=2           \
                                           icon.background.y_offset=-12

</details>

<details>
    <summary>ical.sh</summary>
    
    #!/bin/sh
    
    update() {
        source "$HOME/.config/sketchybar/colors.sh"
        args=()
        SEP="%"
        COUNTER=0
        EVENTS="$(icalBuddy -eed -n -nc -nrd -ea -iep datetime,title -b '' -ps "|${SEP}|" eventsToday)"
        args+=(--remove '/ical.event\.*/')
    
        # Comment this if-section out if you don't want the time of the next event next to the icon
        if [ "${EVENTS}" != "" ]; then
            IFS="${SEP}" read -ra event <<< "$(echo "${EVENTS}" | head -n1)"
            args+=(--set $NAME label="${event[1]}")
        else
            args+=(--set $NAME label="")
        fi
    
        while read -r line; do
            COUNTER=$((COUNTER + 1))
            if [ "${line}" != "" ]; then
                IFS="${SEP}" read -ra event_parts <<< "$line"
                time="${event_parts[1]}"
                title="${event_parts[0]}"
            else
                time="No events today"
                title=":)"
            fi
            args+=(--clone ical.event.$COUNTER ical.template \
                    --set ical.event.$COUNTER label="$title"    \
                    icon="$time"            \
                    icon.color=$YELLOW \
                    click_script="sketchybar --set $NAME popup.drawing=off" \
                    position=popup.ical
            drawing=on)
        done <<< "$(echo "$EVENTS")"
        sketchybar -m "${args[@]}" > /dev/null
        if [ "$SENDER" = "forced" ]; then
            sketchybar --animate tanh 15 --set "$NAME" label.y_offset=5 label.y_offset=0
        fi
    }
    
    popup() {
        sketchybar --set "$NAME" popup.drawing="$1"
    }
    
    case "$SENDER" in
        "routine"|"forced") update
            ;;
        "mouse.entered") popup on
            ;;
        "mouse.exited"|"mouse.exited.global") popup off
            ;;
        "mouse.clicked") popup toggle
            ;;
    esac

</details>



---

Created by [@EphraimSiegfried](https://github.com/EphraimSiegfried)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-4715904)
