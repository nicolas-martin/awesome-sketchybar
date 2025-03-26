# VPN Status

Displays the VPN you're currently connected to, if any.

---

## sketchybarrc
```sh
sketchybar -m --add item vpn right \
              --set vpn icon="" \
                        update_freq=5 \
                        script="~/.config/sketchybar/plugins/vpn.sh"
```

vpn.sh
```
#!/bin/bash

VPN=$(scutil --nc list | grep Connected | sed -E 's/.*"(.*)".*/\1/')
if [[ $VPN != "" ]]; then
  sketchybar -m --set vpn label="$VPN" drawing=on
else
  sketchybar -m --set vpn drawing=off
fi
```
