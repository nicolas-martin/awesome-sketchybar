# Network upload and download speed

https://github.com/user-attachments/assets/89f2bbcd-2e67-4772-aa0a-0353ea54bc09

Shows network upload and download speed, updates every 5 second with a background `netstat` process. Inspired by the network module from https://github.com/Jean-Tinland/simple-bar.

<details>
<summary>
netstat.lua
</summary>

```lua

local function formatBytes(bytes)
	if bytes == 0 then
		return "0b"
	end

	local k = 1024
	local dm = 1
	local sizes = { "b", "kb", "mb", "gb", "tb", "pb", "eb", "zb", "yb" }

	local i = math.floor(math.log(bytes) / math.log(k))

	local value = bytes / (k ^ i)
	if value >= 100 then
		dm = 0
	end
	local formattedValue = string.format("%." .. dm .. "f", value)

	return formattedValue .. " " .. sizes[i + 1]
end

local colors = require("colors")

local netstat_up = sbar.add("item", {
	position = "right",
	width = 66,
	icon = { drawing = false },
	label = { color = colors.blue },
})
sbar.add("item", {
	position = "right",
	icon = {
		string = "",
		font = "JetBrainsMono Nerd Font:Regular:16.0",
		color = colors.blue,
	},
	label = { drawing = false },
})
local netstat_down = sbar.add("item", {
	position = "right",
	width = 66,
	icon = { drawing = false },
	label = { color = colors.red },
})
sbar.add("item", {
	position = "right",
	icon = {
		string = "",
		font = "JetBrainsMono Nerd Font:Regular:16.0",
		color = colors.red,
	},
	label = { drawing = false },
})

sbar.add("event", "netstat_update")

sbar.exec("~/.config/sketchybar/items/netstat.sh")

netstat_up:subscribe("netstat_update", function(env)
	local download = formatBytes(env.DOWNLOAD)
	local upload = formatBytes(env.UPLOAD)
	netstat_up:set({
		label = { string = upload },
	})
	netstat_down:set({
		label = { string = download },
	})
end)

```

</details>

<details>
<summary>
~/.config/sketchybar/items/netstat.sh
</summary>

```bash

#!/usr/bin/env bash

pkill -f "netstat -w5"
netstat -w5 \
  | awk '/[0-9]/ {print $3/5 "," $6/5; fflush(stdout)}' \
  | xargs -I {} bash -c "sketchybar --trigger netstat_update DOWNLOAD=\$(cut -d, -f1 <<< {}) UPLOAD=\$(cut -d, -f2 <<< {})" &

```
</details>

---

Created by [@madmaxieee](https://github.com/madmaxieee)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-10708523)
