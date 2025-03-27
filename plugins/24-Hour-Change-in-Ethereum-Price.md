# 24 Hour Change in Ethereum Price

__DESCRIPTION__: This will display the 24hr change in price of Ethereum in your bar.
__NOTE__: You may need to install requests, or change your python env.

<details>
   <summary>sketchybarrc</summary>

## sketchybarrc
```SHELL
sketchybar -m --add item eth right \
              --set eth icon=ﲹ \
              --set eth update_freq=20 \
              --set eth script="~/.config/sketchybar/plugins/eth.sh"
```
</details>

<details>
   <summary>eth.sh</summary>

```PYTHON
#!/usr/bin/env python3

import requests
import os

response = requests.get('https://api.gemini.com/v1/pricefeed')
jsonResponse = response.json()

for i in jsonResponse:
    if i["pair"] == "BTCUSD":
        percentChange = str(round((float(i["percentChange24h"]) * 100), 2))
        os.system('sketchybar -m --set eth label='+ percentChange + '%')
        break
```
</details>
Felix: Update to v2.0.0 syntax

---

Created by [@typkrft](https://github.com/typkrft)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1216030)
