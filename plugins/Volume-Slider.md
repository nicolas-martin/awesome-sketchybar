# (Volume) Slider
I have included a new event on master (673259897dcf2684bc40f9feca624c8439aefc46) for `volume_change` and have created this demo to show how to create a simple volume slider:
<img width="122" alt="Screen Shot 2022-10-09 at 17 01 41" src="https://user-images.githubusercontent.com/22680421/194764148-7d64e594-1b9d-46d6-87c6-bdb84a63a649.png">


```bash
sketchybar --add slider volume left 100                                           \
           --set volume script="sketchybar --set volume slider.percentage=\$INFO" \
                        slider.highlight_color=0xff8aadf4                         \
                        slider.background.height=10                               \
                        slider.background.corner_radius=3                         \
                        slider.background.color=0xff494d64                        \
           --subscribe volume volume_change
```

---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-3833532)
