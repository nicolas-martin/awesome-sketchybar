# Current space windows
I was highly inspired by arch's window tabs.
<img width="1512" alt="image" src="https://github.com/FelixKratz/SketchyBar/assets/48881444/92ef57b2-509d-4846-85ee-5a7960d80355">

<details>
<summary>apps.lua</summary>

```lua
local colors = require("colors")
local default = require("default")
local settings = require("settings")

local apps = sbar.add("bracket", "apps", {}, {
  position = "left",
  label = {
    font = {
      style = settings.font.style_map["Black"],
      size = 12.0,
    },
  },
})

local function focus_window(env)
  sbar.exec("yabai -m query --windows id,has-focus", function(output)
    for _, line in ipairs(output) do
      sbar.set("apps." .. line.id, {
        label = {
          highlight = line['has-focus'],
        }
      })
    end
  end)
end

local function update_windows(windows)
  -- need to check if exists
  sbar.remove("/apps.\\.*/")
  for _, line in ipairs(windows) do
    width = 655 / #windows
    sbar.add("item", "apps." .. line['id'], {
      label = {
        string = line['app'] .. ": " .. line['title'],
        max_chars = width,
        scroll_duration = 150,
        width = width,
        highlight_color = colors.magenta,
        highlight = line['has-focus'],
      },
      padding_right = 2,
      click_script = "yabai -m window --focus " .. line['id'],
      background = {
        color = colors.black,
        height = 20,
        corner_radius = 0,
        border_width = 1,
        border_color = colors.cyan
      },
    })
  end
end

local function get_apps(env)
  sbar.exec("yabai -m query --windows id,title,app,has-focus --space", update_windows)
end


apps:subscribe("space_change", get_apps)
apps:subscribe("front_app_switched", focus_window)
```
</details>


---

Created by [@weeebdev](https://github.com/weeebdev)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-10023527)
