# 24-Hour Bitcoin Price

Fetches BTC’s 24-hour price change from Gemini’s API.

---

## sketchybarrc
```sh
sketchybar -m --add item btc right \
              --set btc icon="" \
                        update_freq=20 \
                        script="~/.config/sketchybar/plugins/btc.sh"

```
# --- btc.sh ---
```python3
#!/usr/bin/env python3
import requests, os

res = requests.get('https://api.gemini.com/v1/pricefeed').json()
for i in res:
    if i["pair"] == "BTCUSD":
        percentChange = round(float(i["percentChange24h"]) * 100, 2)
        os.system(f'sketchybar -m --set btc label={percentChange}%')
        break
```
