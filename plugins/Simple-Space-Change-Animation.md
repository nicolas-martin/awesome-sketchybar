# Simple Space Change Animation
This is an example for the new animation system, where I animate the space change:

https://user-images.githubusercontent.com/22680421/170866185-fc6fb82c-5cb2-4b03-a991-5727b5637b7f.mp4


(see the comment below for a full minimal and better to understand example)
<details>
   <summary>sketchybarrc</summary>

```bash
sketchybar --add   space          space_template left                \
           --set   space_template icon.highlight_color=0xff9dd274    \
                                  label.drawing=off                  \
                                  drawing=off                        \
                                  updates=on                         \
                                  associated_display=1               \
                                  label.font="$FONT:Black:13.0"      \
                                  icon.font="$FONT:Bold:17.0"        \
                                  script="$PLUGIN_DIR/space.sh"      \
                                  icon.padding_right=6               \
                                  icon.padding_left=2                \
                                  background.padding_left=2          \
                                  background.padding_right=2         \
                                  icon.background.height=2           \
                                  icon.background.color=$ICON_COLOR  \
                                  icon.background.color=$ICON_COLOR  \
                                  icon.background.y_offset=-13       \
                                  click_script="$SPACE_CLICK_SCRIPT" \
                                                                     \
           --clone spaces_1.label label_template                     \
           --set   spaces_1.label label=spc                          \
                                  label.width=38                     \
                                  label.align=center                 \
                                  associated_display=1               \
                                  position=left                      \
                                  drawing=on                         \
                                                                     \
           --clone spaces_1.code  space_template                     \
           --set   spaces_1.code  associated_space=1                 \
                                  icon=􀤙                             \
                                  icon.highlight_color=0xff9dd274    \
                                  icon.background.color=0xff9dd274   \
                                  drawing=on                         \
                                                                     \
           --clone spaces_1.tex   space_template                     \
           --set   spaces_1.tex   associated_space=2                 \
                                  icon=􀓕                             \
                                  icon.highlight_color=0xfff69c5e    \
                                  icon.background.color=0xfff69c5e   \
                                  drawing=on                         \
                                                                     \
           --clone spaces_1.web   space_template                     \
           --set   spaces_1.web   associated_space=3                 \
                                  icon=􀼺                             \
                                  icon.highlight_color=0xff72cce8    \
                                  icon.background.color=0xff72cce8   \
                                  drawing=on                         \
                                                                     \
           --clone spaces_1.idle  space_template                     \
           --set   spaces_1.idle  associated_space=4                 \
                                  icon=􀽎                             \
                                  icon.highlight_color=0xffeacb64    \
                                  icon.background.color=0xffeacb64   \
                                  drawing=on
```
</details> 

<details>
   <summary>space.sh</summary>

```bash
#!/usr/bin/env bash

args=()
if [ "$NAME" != "space_template" ]; then
  args+=(--set $NAME label=$NAME \
                     icon.highlight=$SELECTED)
fi

if [ "$SELECTED" = "true" ]; then
  args+=(--set spaces_$DID.label label=${NAME#"spaces_$DID."})
  args+=(--set $NAME icon.background.y_offset=-12)
else
  args+=(--set $NAME icon.background.y_offset=-20)
fi

sketchybar -m --animate tanh 20 "${args[@]}"
```
</details> 


---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-2843397)
