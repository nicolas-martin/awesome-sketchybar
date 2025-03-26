# VPN Status

__DESCRIPTION__: Shows the vpn your currently connected to if any in your bar.

<details>
   <summary>sketchybarrc</summary>

```SHELL
sketchybar -m --add item vpn right \
              --set vpn icon= \
                        update_freq=5 \
                        script="~/.config/sketchybar/plugins/vpn.sh"
```
</details>

<details>
   <summary>vpn.sh</summary>

```SHELL
#!/bin/bash

VPN=$(scutil --nc list | grep Connected | sed -E 's/.*"(.*)".*/\1/')

if [[ $VPN != "" ]]; then
  sketchybar -m --set vpn icon= \
                          label="$VPN" \
                          drawing=on
else
  sketchybar -m --set vpn drawing=off
fi
```
</details>
Felix: Updated to v2.0.0

---

Created by [@typkrft](https://github.com/typkrft)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1216869)
