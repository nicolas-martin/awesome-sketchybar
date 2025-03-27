### Lowpowermode indicator/toggle

<img width="517" alt="image" src="https://github.com/user-attachments/assets/fa626438-29a0-4391-9682-cf762857ee41">
<img width="523" alt="image" src="https://github.com/user-attachments/assets/9c23c1eb-51b6-4de7-a8e1-c72e832be8af">

Indicator for the low power mode. Red `P` for disable (=power mode) and green `L` for enabled (=low power mode). Requires `SbarLua` and a `sudoers` rule. The latter to toggle between both modes when the indicator is clicked.

First time writing lua, will accept any improvements ;)

<details>
<summary>lowpowermode.lua</summary>

```lua
local colors = {
  green = 0xff9ed072,
  orange = 0xfff39660,
  red = 0xfffc5d7c,
  bg1 = 0xff363944
}

local label = "?"
local color = colors.orange
local modeValue = 0

local lowpowermode = sbar.add("item", "widgets.lowpowermoe", {
  position = "right",
  label = { 
    font = { family = "SF Mono" },
    string = label,
    color = color,
    padding_right = 10
  }
})

local function setModeValue(v)
  modeValue = v
  if v == 1 then
    label = "L"
    color = colors.green
    sbar.exec("sudo pmset -a lowpowermode 1", function() end)
  else
    label = "P"
    color = colors.red
    sbar.exec("sudo pmset -a lowpowermode 0", function() end)
  end
  lowpowermode:set({
    label = { 
      string = label,
      color = color
    },
  })
end

lowpowermode:subscribe({"power_source_change", "system_woke"}, function(env)
  sbar.exec("pmset -g |grep lowpowermode", function(mode_info)
    local found, _, enabled = mode_info:find("(%d+)")
    if found then
      setModeValue(tonumber(enabled))
    end
  end)
end)

lowpowermode:subscribe("mouse.clicked", function(env)
  if modeValue == 0 then
    setModeValue(1)
  else
    setModeValue(0)
  end
end)

sbar.add("bracket", "widgets.lowpowermode.bracket", { lowpowermode.name }, {
  background = { color = colors.bg1 }
})

sbar.add("item", "widgets.lowpowermode.padding", {
  position = "right",
  width = 5
})

```
</details>

<details>
<summary>/private/etc/sudoers.d/lowpowermode</summary>

```
yourUserName ALL=(root) NOPASSWD: /usr/bin/pmset -a lowpowermode 0
yourUserName ALL=(root) NOPASSWD: /usr/bin/pmset -a lowpowermode 1
```
</details>

---

Created by [@bin101](https://github.com/bin101)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-10626991)
