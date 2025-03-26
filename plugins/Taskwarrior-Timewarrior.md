# Taskwarrior & Timewarrior

## Taskwarrior - 3 states:

### No pending task (rest):

<img width="131" alt="task_none" src="https://github.com/FelixKratz/SketchyBar/assets/13052949/17343cf2-84a4-4f9f-91b7-001e26c12feb">

### With **only** pending tasks (_no_ overdue task):

<img width="146" alt="task_pending" src="https://github.com/FelixKratz/SketchyBar/assets/13052949/48d2867f-1c59-449b-916d-14df758e88c5">

### With pending tasks **and** overdue tasks:

<img width="165" alt="task_overdue" src="https://github.com/FelixKratz/SketchyBar/assets/13052949/de03423d-0b97-4fbc-8c6b-782d6aa2b5e0">

## Timewarrior - 3 states:

### Not tracking time (rest):

<img width="131" alt="time_none" src="https://github.com/FelixKratz/SketchyBar/assets/13052949/39f8a0b8-4823-4101-ab3d-a7665828c15e">

### Tracking time but didn't provide any tag:

<img width="143" alt="time_notag" src="https://github.com/FelixKratz/SketchyBar/assets/13052949/774b7b56-b7ad-4d0b-8049-381c20a10806">

### Tracking time with provided tags:

<img width="223" alt="time_withtag" src="https://github.com/FelixKratz/SketchyBar/assets/13052949/d2d7fb2e-e1c7-4971-956d-6149666f67b3">

---

<details><summary>sketchybarrc</summary>

```shell
# Defaults

sketchybar --default padding_left=8                                    \
                     padding_right=8                                   \
                                                                       \
                     background.border_color=$COLOR_YELLOW             \
                     background.border_width=2                         \
                     background.height=40                              \
                     background.corner_radius=12                       \
                                                                       \
                     icon.color=$COLOR_YELLOW                          \
                     icon.highlight_color=$COLOR_BACKGROUND            \
                     icon.padding_left=6                               \
                     icon.padding_right=2                              \
                     icon.font="CaskaydiaCove Nerd Font:Regular:16.0"  \
                                                                       \
                     label.color=$COLOR_YELLOW                         \
                     label.highlight_color=$COLOR_BACKGROUND           \
                     label.padding_left=2                              \
                     label.padding_right=6                             \
                     label.font="CaskaydiaCove Nerd Font:Regular:12.0"

# Items

sketchybar --add item slack left                                     \
           --set      slack script="$PLUGIN_DIR/noti_slack.sh"       \
                           update_freq=60                            \
                           padding_left=8                            \
                           padding_right=2                           \
                           background.border_width=0                 \
                           background.height=24                      \
                           icon=$ICON_CHAT                           \
                           icon.color=$COLOR_WHITE                   \
                           label.color=$COLOR_WHITE                  \
                                                                     \
           --add item mail left                                      \
           --set      mail script="$PLUGIN_DIR/noti_mail.sh"         \
                           update_freq=60                            \
                           padding_left=2                            \
                           padding_right=2                           \
                           background.border_width=0                 \
                           background.height=24                      \
                           icon=$ICON_MAIL                           \
                           icon.color=$COLOR_BLUE                    \
                           label.color=$COLOR_BLUE                   \
                                                                     \
           --add item taskwarrior left                               \
           --set      taskwarrior script="$PLUGIN_DIR/noti_task.sh"  \
                                  update_freq=120                    \
                                  padding_left=2                     \
                                  padding_right=2                    \
                                  background.border_width=0          \
                                  background.height=24               \
                                  icon=$ICON_LIST                    \
                                  icon.color=$COLOR_CYAN             \
                                  label.color=$COLOR_CYAN            \
                                                                     \
           --add item timewarrior left                               \
           --set      timewarrior script="$PLUGIN_DIR/noti_timew.sh" \
                                  update_freq=120                    \
                                  padding_left=2                     \
                                  padding_right=8                    \
                                  background.border_width=0          \
                                  background.height=24               \
                                  icon=$ICON_CLOCK                   \
                                  icon.color=$COLOR_YELLOW           \
                                  label.color=$COLOR_YELLOW

# Bracket

sketchybar --add bracket notifications slack mail taskwarrior timewarrior  \
           --set         notifications background.color=$COLOR_BACKGROUND  \
                                       background.border_color=$COLOR_CYAN
```

</details>

<details><summary>noti_task.sh</summary>

```shell
#!/bin/sh

PENDING_TASK=$(task +PENDING count)
OVERDUE_TASK=$(task +OVERDUE count)

if [[ $PENDING_TASK == 0 ]]; then
  sketchybar --set $NAME label.drawing=off    \
                         icon.padding_left=4  \
                         icon.padding_right=4
else
  if [[ $OVERDUE_TASK == 0 ]]; then
    LABEL=$PENDING_TASK
  else
    LABEL="!$OVERDUE_TASK/$PENDING_TASK"
  fi

  sketchybar --set $NAME label="${LABEL}"     \
                         label.drawing=on     \
                         icon.padding_left=6  \
                         icon.padding_right=2
fi
```

</details>

<details><summary>noti_timew.sh</summary>

```shell
#!/bin/sh

IS_ACTIVE=$(timew get dom.active)

if [[ $IS_ACTIVE == 0 ]]; then
  sketchybar --set $NAME label.drawing=off    \
                         icon.padding_left=4  \
                         icon.padding_right=4
else
  HAS_TAGS=$(timew get dom.active.tag.count)

  if [[ $HAS_TAGS == 0 ]]; then
    LABEL="•"
  else
    TAGS=$(timew get dom.active.json | jq -r '.tags[]')
    TAGS_ARR=()
    while read -r line; do TAGS_ARR+=("$line"); done <<< "$TAGS"
    DELIM=""
    LABEL=""
    for TAG in "${TAGS_ARR[@]}";
    do
      LABEL="$LABEL$DELIM$TAG"
      DELIM=", "
    done
    LABEL=$(echo "$LABEL" | cut -c 1-25)
  fi

  sketchybar --set $NAME label="${LABEL}"     \
                         label.drawing=on     \
                         icon.padding_left=6  \
                         icon.padding_right=2
fi
```

</details> 

---

Created by [@hbthen3rd](https://github.com/hbthen3rd)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-6974137)
