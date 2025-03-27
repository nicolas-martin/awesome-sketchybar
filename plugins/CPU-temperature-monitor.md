#  CPU temperature monitor

### Description
This is a plugin that can show your cpu temperature

![image](https://github.com/FelixKratz/SketchyBar/assets/50800571/ada621ba-3a1d-4f08-84a6-6935d1adb0db)
### Note
You need to install [`smctemp` ](https://github.com/narugit/smctemp)




<details>
<summary>iterms/temperature</summary>

```bash
#!/bin/bash

temperature=(
    script="$PLUGIN_DIR/temperature.sh"
    icon=󱤋
    icon.color=$WHITE
    icon.font="$FONT:Regular:19.0"
    icon.padding_right=0
    padding_right=0
    padding_left=0
    # label.drawing=on
    label.padding_right=5
    update_freq=10
)
sketchybar --add item temperature right\
    --set temperature "${temperature[@]}" 
```
</details>


<details>
<summary>plugins/temperature</summary>

```bash
#!/bin/bash

# get temperature
TEMPERATURE=$(/usr/local/bin/smctemp -c)  # you can use (which smctemp) to replace this path if your install path is different

# check smctemp whether running well
if [ $? -ne 0 ]; then
    echo "Error: Unable to get temperature."
    exit 1
fi

# update sketchybar shown
sketchybar --set $NAME temperature label="${TEMPERATURE}󰔄"

```
</details>






---

Created by [@xbunax](https://github.com/xbunax)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-8011303)
