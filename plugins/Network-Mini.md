# Network Mini
Small widget for displaying upload and download speeds. Needs ifstat to be installed.

<details>
  <summary>sketchybarrc</summary>

```bash
sketchybar -m --add       item               network_up left                                               \
              --set       network_up         label.font="SF Pro:Heavy:9"                                   \
                                             icon.font="SF Pro:Heavy:9"                                    \
                                             icon=                                                         \
                                             icon.highlight_color=0xff8b0a0d                               \
                                             y_offset=5                                                    \
                                             width=0                                                       \
                                             update_freq=2                                                 \
                                             script="~/.config/sketchybar/plugins/network.sh"              \
                                                                                                           \
              --add       item               network_down left                                             \
              --set       network_down       label.font="SF Pro:Heavy:9"                                   \
                                             icon.font="SF Pro:Heavy:9"                                    \
                                             icon=                                                         \
                                             icon.highlight_color=0xff10528c                               \
                                             y_offset=-5
```
</details>

<details>
  <summary>network.sh</summary>

```bash
#!/usr/bin/env bash
UPDOWN=$(ifstat -i "en0" -b 0.1 1 | tail -n1)
DOWN=$(echo $UPDOWN | awk "{ print \$1 }" | cut -f1 -d ".")
UP=$(echo $UPDOWN | awk "{ print \$2 }" | cut -f1 -d ".")

DOWN_FORMAT=""
if [ "$DOWN" -gt "999" ]; then
  DOWN_FORMAT=$(echo $DOWN | awk '{ printf "%.0f Mbps", $1 / 1000}')
else
  DOWN_FORMAT=$(echo $DOWN | awk '{ printf "%.0f kbps", $1}')
fi

UP_FORMAT=""
if [ "$UP" -gt "999" ]; then
  UP_FORMAT=$(echo $UP | awk '{ printf "%.0f Mbps", $1 / 1000}')
else
  UP_FORMAT=$(echo $UP | awk '{ printf "%.0f kbps", $1}')
fi

sketchybar -m --set network_down label="$DOWN_FORMAT" icon.highlight=$(if [ "$DOWN" -gt "0" ]; then echo "on"; else echo "off"; fi) \
                    --set network_up label="$UP_FORMAT" icon.highlight=$(if [ "$UP" -gt "0" ]; then echo "on"; else echo "off"; fi)
```
</details>

## Example
![Screen Shot 2021-09-24 at 00 46 11](https://user-images.githubusercontent.com/22680421/134595114-c87885e3-bd91-4929-b2ca-a8ea26d3b822.jpg)



---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1377134)
