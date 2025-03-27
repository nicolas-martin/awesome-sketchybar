# Wifi Indicator
Small Item displaying the current SSID and TX rate of wifi:
<img width="171" alt="Screen Shot 2022-07-01 at 17 36 00" src="https://user-images.githubusercontent.com/22680421/176925891-ccfbea39-ead4-4442-94dd-763ff05a0f19.png">


<details>
   <summary>sketchybarrc</summary>

```bash
sketchybar --add item wifi right                         \
           --set wifi    script="$PLUGIN_DIR/wifi.sh"    \
                         background.padding_right=12     \
                         update_freq=5
```
</details> 

<details>
   <summary>wifi.sh</summary>

```bash
#!/usr/bin/env sh

CURRENT_WIFI="$(/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -I)"
SSID="$(echo "$CURRENT_WIFI" | grep -o "SSID: .*" | sed 's/^SSID: //')"
CURR_TX="$(echo "$CURRENT_WIFI" | grep -o "lastTxRate: .*" | sed 's/^lastTxRate: //')"

if [ "$SSID" = "" ]; then
  sketchybar --set $NAME label="Disconnected" icon=睊
else
  sketchybar --set $NAME label="$SSID (${CURR_TX}Mbps)" icon=直
fi
```
</details>


---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-3065175)
