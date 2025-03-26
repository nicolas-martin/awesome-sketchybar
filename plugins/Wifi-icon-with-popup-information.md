## Wifi icon with popup information
<img width="343" alt="image" src="https://user-images.githubusercontent.com/1778670/211180214-b3ab042b-7ebe-4cb3-b08e-e3e4fd27f098.png">

<details>
  <summary>sketchybarrc</summary>

### Item
```bash
#!/usr/bin/env sh

POPUP_CLICK_SCRIPT="sketchybar --set $NAME popup.drawing=toggle"

sketchybar --add alias  "Control Center,WiFi" right                      \
           --rename     "Control Center,WiFi" wifi.alias                 \
           --set        wifi.alias    icon.drawing=off                   \
                                      alias.color="$YELLOW"              \
                                      background.padding_right=0         \
                                      icon.padding_right=0               \
                                      align=right                        \
                                      click_script="$POPUP_CLICK_SCRIPT" \
                                      script="$PLUGIN_DIR/wifi.sh"       \
                                      update_freq=1                      \
           --subscribe  wifi.alias    mouse.entered                      \
                                      mouse.exited                       \
                                      mouse.exited.global                \
                                                                         \
            --add       item          wifi.details popup.wifi.alias      \
            --set       wifi.details  background.corner_radius=12        \
                                      background.padding_left=5          \
                                      background.padding_right=10        \
                                      icon.background.height=2           \
                                      icon.background.y_offset=-12       \
                                      label.align=center                 \
                                      click_script="sketchybar --set wifi.alias popup.drawing=off"
```
</details>

<details>
  <summary>wifi.sh</summary>

### Plugin
```bash
#!/usr/bin/env bash

source "$HOME/.config/sketchybar/colors.sh"
source "$HOME/.config/sketchybar/icons.sh"

render_bar_item() {
    if [ "$SSID" = "" ]; then
        args+=(--set "$NAME" label="N/A")
    else
        args+=(--set "$NAME"    label="$SSID (${CURR_TX}Mbps)" \
                                label.drawing=off) # remove if you want more detailed info available without hovering
    fi

}

render_popup() {
   args+=(--set wifi.details label="$SSID ($CURR_TX Mbps)"                            \
                            click_script="sketchybar --set $NAME popup.drawing=off")

  sketchybar -m "${args[@]}" > /dev/null

}

update() {
  CURRENT_WIFI="$(/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -I)"
  SSID="$(echo "$CURRENT_WIFI" | grep -o "SSID: .*" | sed 's/^SSID: //')"
  CURR_TX="$(echo "$CURRENT_WIFI" | grep -o "lastTxRate: .*" | sed 's/^lastTxRate: //')"

  args=()

  render_bar_item
  render_popup
  
  if [ "$SENDER" = "forced" ]; then
    sketchybar --animate tanh 15 --set "$NAME" label.y_offset=5 label.y_offset=0
  fi
}

popup() {
  sketchybar --set "$NAME" popup.drawing="$1"
}

case "$SENDER" in
  "routine"|"forced") update
  ;;
  "mouse.entered") popup on
  ;;
  "mouse.exited"|"mouse.exited.global") popup off
  ;;
  "mouse.clicked") popup toggle
  ;;
esac
```
</details>

---

Created by [@khaneliman](https://github.com/khaneliman)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-4623276)
