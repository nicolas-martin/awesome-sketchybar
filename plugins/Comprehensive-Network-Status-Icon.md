# Comprehensive Network Status Icon

**DESCRIPTION:** A simple network status icon that recognizes different network devices used for connection, as well as distinguishes between disconnected and off states for Wi-Fi.
Currently supports Wi-Fi, iPhone Wi-Fi hotspot, iPhone USB hotspot, and Ethernet Thunderbolt bridge.
Doesn't use `airport` so should work with latest Sonoma and hopefully future releases for the foreseeable future.

**NOTES:**
- Other connections, such as Ethernet connections outside of Thunderbolt bridge, should be easy to support by adding more cases to the `$service` switch, matching devices you see in `networksetup -listnetworkserviceorder`).
- Some of the icons are lacking inspiration since I kept to SF Symbols :)

**ISSUES:**
- iPhone hotspot checking is _very_ rudimentary, relying on "iPhone" being part of the SSID, which can easily be fooled in both directions. There are much more reliable [methods](https://apple.stackexchange.com/a/457626) for deducing this, but they seemed a bit of an overkill for my use case.
- Turning Wi-Fi off while not connected to a Wi-Fi network doesn't trigger the `wifi_change` event so the icon won't update. It could also be the case that some other transitions act the same, I didn't test this outside of my own day-to-day use cases. This could easily be fixed by setting `update_freq` if it's important to you, or even writing a custom native helper if you're up for a challenge.

<details>
  <summary>plugins/net.sh</summary>

```sh
#!/usr/bin/env sh

. "$CONFIG_DIR/resources.sh"

# When switching between devices, it's possible to get hit with multiple
# concurrent events, some of which may occur before `scutil` picks up the
# changes, resulting in race conditions.
sleep 1

services=$(networksetup -listnetworkserviceorder)
device=$(scutil --nwi | sed -n "s/.*Network interfaces: \([^,]*\).*/\1/p")

test -n "$device" && service=$(echo "$services" \
  | sed -n "s/.*Hardware Port: \([^,]*\), Device: $device.*/\1/p")

color=$FG1
case $service in
  "iPhone USB")         icon=$NET_USB;;
  "Thunderbolt Bridge") icon=$NET_THUNDERBOLT;;

  Wi-Fi)
    ssid=$(networksetup -getairportnetwork "$device" \
      | sed -n "s/Current Wi-Fi Network: \(.*\)/\1/p")
    case $ssid in
      *iPhone*) icon=$NET_HOTSPOT;;
      "")       icon=$NET_DISCONNECTED; color=$FG2;;
      *)        icon=$NET_WIFI;;
    esac;;

  *)
    wifi_device=$(echo "$services" \
      | sed -n "s/.*Hardware Port: Wi-Fi, Device: \([^\)]*\).*/\1/p")
    test -n "$wifi_device" && status=$( \
      networksetup -getairportpower "$wifi_device" | awk '{print $NF}')
    icon=$(test "$status" = On && echo "$NET_DISCONNECTED" || echo "$NET_OFF")
    color=$FG2;;
esac

sketchybar --animate sin 5 --set "$NAME" icon="$icon" icon.color="$color"
```
</details>

<details>
  <summary>resources.sh</summary>

```sh
#!/usr/bin/env sh

# From Gruvbox Dark
FG1=0xffebdbb2 # or whatever you use for active icons
FG2=0xff665c54 # or whatever you use for dim icons

# From SF Symbols
NET_WIFI=􀙇         # Wi-Fi connected
NET_HOTSPOT=􀉤      # iPhone Wi-Fi hotspot connected
NET_USB=􀟜          # iPhone USB hotspot connected
NET_THUNDERBOLT=􀒗  # Thunderbolt bridge connected
NET_DISCONNECTED=􀙇 # Network disconnected, but Wi-Fi turned on
NET_OFF=􀙈          # Network disconnected, Wi-Fi turned off
```
</details>

<details>
  <summary>sketchybarrc</summary>

```sh
# ...
sketchybar --add item net right                  \
           --set net script="$PLUGIN_DIR/net.sh" \
                     updates=on                  \
                     label.drawing=off           \
           --subscribe net wifi_change
# ...
```
</details>

---

Created by [@toxxmeister](https://github.com/toxxmeister)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-8908932)
