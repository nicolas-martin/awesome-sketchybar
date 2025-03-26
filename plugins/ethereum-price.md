# 24-Hour Ethereum Price

Similar to the BTC plugin, but displays ETH’s 24-hour change.

---

## sketchybarrc
```sh
sketchybar -m --add item eth right \
              --set eth icon="ﲹ" \
                        update_freq=20 \
                        script="~/.config/sketchybar/plugins/eth.sh"
