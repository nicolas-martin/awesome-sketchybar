# Bluetooth Headphone Indicator
There is nothing more painful than to be in the office and start music on full volume via speaker...
This small widget displays if a bluetooth headphone is connected (Works for my Bose QC35). 

It hooks onto the system bluetooth notification and is thus very lightweight and responsive.
![Screen Shot 2021-10-12 at 17 47 53](https://user-images.githubusercontent.com/22680421/136989037-888c3b0f-7b7b-47c0-907f-dfea6ac38ab1.png)
<details>
   <summary>sketchybarrc</summary>

Add the icon to your liking.
```bash
sketchybar -m --add       event              bluetooth_change "com.apple.bluetooth.status"                       \
                                                                                                                 \
              --add       item headphones    right                                                               \
              --set       headphones         icon=insert_icon                                                    \
                                             script="~/.config/sketchybar/plugins/ble_headset.sh"                \
              --subscribe headphones         bluetooth_change

```
</details>

<details>
  <summary>ble_headset.sh</summary>

```bash
#!/usr/bin/env bash

DEVICES="$(system_profiler SPBluetoothDataType -json -detailLevel basic 2>/dev/null | jq '.SPBluetoothDataType' | jq '.[0]' | jq '.device_title' | jq -r '.[] | keys[] as $k | "\($k) \(.[$k] | .device_isconnected) \(.[$k] | .device_minorClassOfDevice_string)"' | grep 'attrib_Yes' | grep 'Headphones')"

if [ "$DEVICES" = "" ]; then
  sketchybar -m --set $NAME drawing=off
else
  sketchybar -m --set $NAME drawing=on
fi
```
</details>

---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1465761)
