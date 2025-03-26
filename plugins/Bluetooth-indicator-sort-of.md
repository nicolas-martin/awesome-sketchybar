## Bluetooth indicator (sort of)

<img width="252" alt="image" src="https://user-images.githubusercontent.com/2601363/210251472-b02e5ea8-fc1a-4817-b4d9-e64597c66116.png">

<details>
  <summary>sketchybarrc</summary>

```bash
sketchybar  -m --add event bluetooth_change "com.apple.bluetooth.status" \
            --add item bluetooth right                                   \
            --set bluetooth script="$PLUGIN_DIR/bluetooth.sh"            \
                           click_script="$PLUGIN_DIR/bt_click.sh"        \
                           update_freq=5                                 \
            --subscribe bluetooth bluetooth_change
```
</details>

<details>
  <summary>bluetooth.sh</summary>

```bash
#!/usr/bin/env sh

STATE=$(blueutil -p)

if [ $STATE = 0 ]; then
  sketchybar --set $NAME label="" icon=􁅒
else
  sketchybar --set $NAME label="" icon=􀖀
fi
```
</details>

<details>
  <summary>bt_click.sh</summary>

```bash
#!/usr/bin/env sh

STATE=$(blueutil -p)


if [ "$STATE" = "0" ]; then
  blueutil -p 1
else
  blueutil -p 0
fi
```
</details>

Notes:
1. uses [blueutil](https://github.com/toy/blueutil). I tried using regular commands but I was not able.
2. fs-symbols, does not have a valid "bluetooth" icon, maybe, because of licensing.


---

Created by [@ricbermo](https://github.com/ricbermo)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-4572652)
