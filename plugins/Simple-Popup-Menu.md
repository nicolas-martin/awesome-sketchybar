# Simple Popup Menu
This is a demo of the new `popup` windows:
This is the default look:
<img width="224" alt="Screen Shot 2021-12-19 at 20 22 29" src="https://user-images.githubusercontent.com/22680421/146688037-767adbf1-4a16-435d-ad11-0fce08396459.png">

after i click the apple logo:
<img width="224" alt="Screen Shot 2021-12-19 at 20 31 29" src="https://user-images.githubusercontent.com/22680421/146688291-b8bc5e77-e6a2-42ee-bd9f-b3709c63d936.png">


<details>
  <summary>sketchybarrc</summary>

```bash
#!/usr/bin/env bash

sketchybar -m --bar blur_radius=50                                                            \
                    height=32                                                                 \
              --add item apple.logo left                                                      \
              --set apple.logo icon=􀣺                                                         \
                               icon.font="SF Pro:Black:16.0"                                  \
                               label.drawing=off                                              \
                               click_script="sketchybar -m --set \$NAME popup.drawing=toggle" \
                               popup.background.border_width=2                                \
                               popup.background.corner_radius=3                                \
                               popup.background.border_color=0xff9dd274                       \
                                                                                              \
              --default background.padding_left=5                                             \
                        background.padding_right=5                                            \
                        icon.padding_right=5                                                  \
                        icon.font="SF Pro:Bold:16.0"                                          \
                        label.font="SF Pro:Semibold:13.0"                                     \
                                                                                              \
              --add item apple.preferences popup.apple.logo                                   \
              --set apple.preferences icon=􀺽                                                  \
                               label="Preferences"                                            \
                               click_script="open -a 'System Preferences';                    
                                             sketchybar -m --set apple.logo popup.drawing=off"\
              --add item apple.activity popup.apple.logo                                      \
              --set apple.activity icon=􀒓                                                     \
                               label="Activity"                                               \
                               click_script="open -a 'Activity Monitor';                       
                                             sketchybar -m --set apple.logo popup.drawing=off"\
              --add item apple.lock popup.apple.logo                                          \
              --set apple.lock icon=􀒳                                                         \
                               label="Lock Screen"                                            \
                               click_script="pmset displaysleepnow;                           
                                             sketchybar -m --set apple.logo popup.drawing=off"

```
</details>

---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1843975)
