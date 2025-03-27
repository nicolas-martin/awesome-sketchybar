## Airpods Battery ##

#### Description ####

Used [FexliKratz’s](https://github.com/FelixKratz) Bluetooth Headphone Indicator plugin as a jumping off point and made a plugin to display the battery percentage of left and right airpods and the case.

<img width="91" alt="Screen Shot 2021-10-27 at T20 09 27" src="https://user-images.githubusercontent.com/53032923/139169018-b2f1ee22-6cb3-4c9a-a111-edcd2d53e142.png">

#### Known Issues ####

The case battery only shows up when the airpods are in the case. This means you would normally have an ugly `0` in the middle of your plugin. To get around this, I added a bit of code to display items with 0% battery life as a hyphen `-` instead to make it look a little nicer.

Also, this script could probably be modified pretty easily to work with the AirPods Max, but I don’t have any on hand to test with.

<details><summary>sketchybarrc</summary><p>

```
sketchybar -m --add event bluetooth_change "com.apple.bluetooth.status" \
              --add item headphones right \
              --set headphones icon=" " \
              script="~/.config/sketchybar/plugins/airpods_battery.sh" \
              --subscribe headphones bluetooth_change
```

</p></details>

<details><summary>airpods_battery.sh</summary><p>

```
DEVICES="$(system_profiler SPBluetoothDataType -json -detailLevel basic 2>/dev/null | jq '.SPBluetoothDataType' | jq '.[0]' | jq '.device_title' | jq -r '.[] | keys[] as $k | "\($k) \(.[$k] | .device_isconnected) \(.[$k] | .device_minorClassOfDevice_string)"' | grep 'attrib_Yes' | grep 'Headphones')"

if [ "$DEVICES" = "" ]; then
  sketchybar -m --set $NAME drawing=off
else
  sketchybar -m --set $NAME drawing=on
  # Left
  LEFT="$(defaults read /Library/Preferences/com.apple.Bluetooth | grep BatteryPercentLeft | tr -d \; | awk '{print $3}')"

  # Right
  RIGHT="$(defaults read /Library/Preferences/com.apple.Bluetooth | grep BatteryPercentRight | tr -d \; | awk '{print $3}')"

  # Case
  CASE="$(defaults read /Library/Preferences/com.apple.Bluetooth | grep BatteryPercentCase | tr -d \; | awk '{print $3}')"

  if [ $LEFT = 0 ]; then
    LEFT="-"
  fi

  if [ $RIGHT = 0 ]; then
    RIGHT="-"
  fi

  if [ $CASE -eq 0 ]; then
    CASE="-"
  fi
  
  sketchybar -m --set $NAME label="$LEFT $CASE $RIGHT"
  echo "$LEFT $CASE $RIGHT"
fi
```

</p></details>

---

Created by [@SxC97](https://github.com/SxC97)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1549450)
