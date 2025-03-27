### Volume warning
Display a warning if headphones are connected and the volume percentage is set higher than a prescribed value. The higher the percentage, the redder the color. For a percentage higher than 50% an additional warning sign is displayed. Every 30 seconds, there is a little jump animation to remind you.

**This needs the recent addition of the ```volume_change``` event (version 2.10.0+)**

<img width="80" alt="grafik" src="https://user-images.githubusercontent.com/50205381/195553733-6b8ac3f9-9be3-4713-ae38-49a919141d25.png"> <img width="77" alt="grafik" src="https://user-images.githubusercontent.com/50205381/195553942-3839d7c7-7990-4f03-82db-ea914bcff047.png"> <img width="95" alt="grafik" src="https://user-images.githubusercontent.com/50205381/195554225-4231c172-08d3-49a9-bbe0-755dadf6f165.png">

<details>
<summary>sketchybarrc</summary>
<br>

```
#!/usr/bin/env sh
sketchybar --add       event              volume_change                                               \
           --add       item               tooloud right                                               \
           --set       tooloud            update_freq=10                                              \
                                          label.padding_right=5                                       \
                                          label.padding_left=0                                        \
                                          background.padding_left=24                                  \
                                          background.color=0x33ffffff                                 \
                                          background.corner_radius=5                                  \
                                          background.height=20                                        \
                                          background.padding_right=1                                  \
                                          icon.font="$ICON_FONT:Regular:14.0"                         \
                                          icon.padding_left=5                                         \
                                          icon.padding_right=5                                        \
                                          script="$PLUGIN_DIR/tooloud.sh"                             \
           --subscribe tooloud            volume_change
```

</details>
<details>
<summary>tooloud.sh</summary>
<br>

```
#!/usr/bin/env bash

if system_profiler SPAudioDataType | grep --quiet "External Headphones"; then
    VOLUME=$(osascript -e "get volume settings" | cut -d " " -d ":" -f2 | cut -d "," -f1)
    
    if [ $VOLUME -gt 30 ]; then
        COLOR=0xffffd152
        ICON=""
        LCOLOR=0x88000000
        if [ $VOLUME -gt 37 ]; then
            COLOR=0xfffcb462
            if [ $VOLUME -gt 40 ]; then
                COLOR=0xfffc9562
                if [ $VOLUME -gt 45 ]; then
                    COLOR=0xffe8555b
                    ICON=" $ICON"
                    LCOLOR=0xffffffff
                fi
            fi
        fi
        sketchybar -m --animate lin 5                           \
                      --set $NAME   background.drawing=on       \
                                    background.color="$COLOR"   \
                                    label.drawing=on            \
                                    label="$VOLUME%"            \
                                    label.color="$LCOLOR"       \
                                    icon="$ICON"                \
                                    icon.color="$LCOLOR"        \
                                    icon.drawing=on             \
                                    y_offset=3                  \
                                    y_offset=0
    else
        sketchybar -m --set $NAME label.drawing=off icon.drawing=off background.drawing=off
    fi
else
    sketchybar -m --set $NAME label.drawing=off icon.drawing=off background.drawing=off
fi 

```
</details>



---

Created by [@akarsai](https://github.com/akarsai)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-3868788)
