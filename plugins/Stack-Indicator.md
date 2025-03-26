# Stack Indicator

__DESCRIPTION__: Show you Window's Stack Position.

__NOTE__: Occasionally querying yabai can be delayed possibly related to [this](https://github.com/koekeishiya/yabai/issues/626#issuecomment-667569205), which can lead to a number or dot being displayed late and overwriting the actual position in the indicator.

<details>
   <summary>sketchybarrc</summary>

```SHELL
# S T A C K  I N D I C A T O R 
sketchybar -m --add item stack_sep                       center \
              --add item stack                           center \
              --set stack script="$HOME/.dotfiles/sketchybar/plugins/stack.sh" \
              --subscribe stack window_focus front_app_switched space_change title_change \
              --set stack_sep drawing=off \
              --set stack drawing=off \
              --set stack update_freq=0
```
</details>

<details>
   <summary>stack.sh</summary>

```SHELL
#!/bin/bash

# Exit if Not in Stack
CURRENT=$(yabai -m query --windows --window | jq '.["stack-index"]')
if [[ $CURRENT -eq 0 ]]; then
  sketchybar -m --set stack label="" \
                --set stack_sep drawing=off \
                --set stack drawing=off
  exit 0
fi

# Use Numbers in place of Dots if the Stack is greater than 10
# Use a larger font for the unicode dots
LAST=$(yabai -m query --windows --window stack.last | jq '.["stack-index"]')
if [[ $LAST -gt 10 ]]; then
  sketchybar -m --set stack label.font="Iosevka Nerd Font:Bold:16.0" \
                --set stack label=$(printf "[%s/%s]" "$CURRENT" "$LAST") \
                --set stack_sep drawing=on \
                --set stack drawing=on
  exit 0
else
  sketchybar -m --set stack label.font="Iosevka Nerd Font:Bold:22.0"
fi

# Create Stack Indicator
declare -a dots=()
for i in $(seq 0 $(expr $LAST - 1))
do  
  # Theme 1
  # if [[ $i -lt $(expr $CURRENT - 1) ]]; then
    # dots+="◖"
  # elif [[ $i -gt $(expr $CURRENT - 1) ]]; then
    # dots+="◗"
  # elif [[ $i -eq $(expr $CURRENT - 1) ]]; then
    # dots+="●"
  # fi
  # Theme 2
  [[ $( expr $CURRENT - 1) -eq $i ]] && dots+="●" || dots+="○"
done

# Display Indicator
sketchybar -m --set stack label=$(printf "%s" ${dots[@]}) \
              --set stack_sep drawing=on \
              --set stack drawing=on
```
</details>
Felix: Update to v2.0.0

---

Created by [@typkrft](https://github.com/typkrft)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1263547)
