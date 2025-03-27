# Used Swap

<img width="290" alt="skb_swap" src="https://github.com/FelixKratz/SketchyBar/assets/13052949/aac2b7ac-786c-4aad-bbe7-4a75c43b1ae3">

<details><summary>sketchybarrc</summary>

```shell
# Bar

sketchybar --bar color=0x00000000 \
                 height=56        \
                 margin=0         \
                 y_offset=0       \
                 padding_left=8   \
                 padding_right=8

# Defaults

sketchybar --default padding_left=8                                \
                     padding_right=8                               \
                                                                   \
                     background.border_color=$YELLOW               \
                     background.border_width=2                     \
                     background.height=40                          \
                     background.corner_radius=12                   \
                                                                   \
                     icon.color=$YELLOW                            \
                     icon.highlight_color=$BACKGROUND              \
                     icon.padding_left=6                           \
                     icon.padding_right=2                          \
                     icon.font="CaskaydiaCove Nerd Font:Book:16.0" \
                                                                   \
                     label.color=$YELLOW                           \
                     label.highlight_color=$BACKGROUND             \
                     label.padding_left=2                          \
                     label.padding_right=6                         \
                     label.font="SF Pro:Semibold:12.0"

# Items

sketchybar --add item  swap right                        \
           --set       swap script="$PLUGIN_DIR/swap.sh" \
                            update_freq=60               \
                            padding_left=2               \
                            padding_right=8              \
                            background.border_width=0    \
                            background.height=24         \
                            icon=                       \
                            icon.color=$CYAN             \
                            label.color=$CYAN            \
                                                         \
           --add item  ram right                         \
           --set       ram script="$PLUGIN_DIR/ram.sh"   \
                           update_freq=60                \
                           padding_left=2                \
                           padding_right=2               \
                           background.border_width=0     \
                           background.height=24          \
                           icon=                        \
                           icon.color=$GREEN             \
                           label.color=$GREEN            \
                                                         \
           --add item  disk right                        \
           --set       disk script="$PLUGIN_DIR/disk.sh" \
                            update_freq=60               \
                            padding_left=2               \
                            padding_right=2              \
                            background.border_width=0    \
                            background.height=24         \
                            icon=                       \
                            icon.color=$YELLOW           \
                            label.color=$YELLOW          \
                                                         \
           --add item  cpu right                         \
           --set       cpu script="$PLUGIN_DIR/cpu.sh"  \
                           update_freq=60                \
                           padding_left=8                \
                           padding_right=2               \
                           background.border_width=0     \
                           background.height=24          \
                           icon=                        \
                           icon.color=$RED               \
                           label.color=$RED

# Bracket

sketchybar --add bracket activity swap ram disk cpu                \
           --set         activity background.color=$BACKGROUND     \
                                  background.border_color=$MAGENTA
```

</details>

<details><summary>swap.sh</summary>

```shell
#!/bin/sh

sketchybar --set "$NAME" label=$(echo $(sysctl vm.swapusage) | awk '/vm.swapusage: / { print substr($7, 1, length($7)) }')
```

</details> 

---

Created by [@hbthen3rd](https://github.com/hbthen3rd)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-6793085)
