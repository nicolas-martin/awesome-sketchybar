# Simple Lock/Unlock Animation
This is a further example for the new animation system, where I animate the
bar on a screen unlock event, i.e. this animation is always performed after
entering the users password. It uses key-frames to concatenate a number of
animations:

(This is on a notched MacBook, the center gap will not be visible on external displays or can be disabled with `notch_width=0`)

https://user-images.githubusercontent.com/22680421/174477453-56eeb31a-7479-4aeb-a2d1-b0f25380f6b0.mp4

<details>
   <summary>sketchybarrc</summary>

```bash
sketchybar --add event lock   "com.apple.screenIsLocked"   \
           --add event unlock "com.apple.screenIsUnlocked" \
                                                           \
           --add item         animator left                \
           --set animator     drawing=off                  \
                              updates=on                   \
                              script="$PLUGIN_DIR/wake.sh" \
           --subscribe        animator lock unlock
```
</details> 

<details>
   <summary>wake.sh</summary>

```bash
#!/usr/bin/env sh

BAR_COLOR=0x15ffffff

lock() {
  sketchybar --bar y_offset=-32 \
                   margin=-200 \
                   notch_width=0 \
                   blur_radius=0 \
                   color=0x000000
}

unlock() {
  sketchybar --animate sin 25 \
             --bar y_offset=0 \
                   notch_width=200 \
                   margin=0 \
                   shadow=on \
                   corner_radius=20 \
                   corner_radius=20 \
                   corner_radius=20 \
                   corner_radius=0 \
                   color=0x000000 \
                   color=0x000000 \
                   color=$BAR_COLOR \
                   blur_radius=0 \
                   blur_radius=0 \
                   blur_radius=0 \
                   blur_radius=50
}

case "$SENDER" in
  "lock") lock
  ;;
  "unlock") unlock
  ;;
esac
```
</details> 

---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-2979651)
