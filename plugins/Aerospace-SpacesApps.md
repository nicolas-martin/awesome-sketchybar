### Aerospace Spaces/Apps

![image](https://github.com/user-attachments/assets/ce74a43b-af40-443e-8744-25d314a0f272)

**!!!IMPORTANT!!!**

Aerospace v0.14.2-Beta don't have some bugfixes which are already included in the current main branch of aerospace which are mandatory to let the plugin work correctly!

I used the yabai implementation from Felix as a base (https://github.com/FelixKratz/dotfiles/blob/master/.config/sketchybar/items/spaces.lua) so `colors.lua`, `icons.lua` etc. are mandatory requirements. Or adjust it for yourself.

I named my aerospace workspaces with a number prefix (1_{workspace_name} .. n-th_{workspace_name}) because aerospace workspace query outputs them by alphanumeric ordering. I then strip the prefix inside the plugin and because the async behaviour of `sbar.exec("aerospace list-workspaces --all", function(spaces)` I had to reorder the sketchybar items in the second to last line `sbar.exec("sketchybar --reorder apple " .. item_order .. " front_app")`

Don't forget to add the event trigger for workspace changes inside the `aerospace.toml`:
```
exec-on-workspace-change = ['/bin/bash', '-c',
    'sketchybar --trigger aerospace_workspace_change FOCUSED_WORKSPACE=$AEROSPACE_FOCUSED_WORKSPACE',
]
```

<details>
<summary>spaces.lua</summary>

```lua
local colors = require("colors")
local icons = require("icons")
local settings = require("settings")
local app_icons = require("helpers.app_icons")

local item_order = ""

sbar.exec("aerospace list-workspaces --all", function(spaces)
  for space_name in spaces:gmatch("[^\r\n]+") do
    local space = sbar.add("item", "space." .. space_name, {
      icon = {
        font = { family = settings.font.numbers },
        string = string.sub(space_name, 3),
        padding_left = 7,
        padding_right = 3,
        color = colors.white,
        highlight_color = colors.red,
      },
      label = {
        padding_right = 12,
        color = colors.grey,
        highlight_color = colors.white,
        font = "sketchybar-app-font:Regular:16.0",
        y_offset = -1,
      },
      padding_right = 1,
      padding_left = 1,
      background = {
        color = colors.bg1,
        border_width = 1,
        height = 26,
        border_color = colors.black,
      }
    })
  
    local space_bracket = sbar.add("bracket", { space.name }, {
      background = {
        color = colors.transparent,
        border_color = colors.bg2,
        height = 28,
        border_width = 2
      }
    })
  
    -- Padding space
    local space_padding = sbar.add("item", "space.padding." .. space_name, {
      script = "",
      width = settings.group_paddings,
    })
  
    space:subscribe("aerospace_workspace_change", function(env)
      local selected = env.FOCUSED_WORKSPACE == space_name
      local color = selected and colors.grey or colors.bg2
      space:set({
        icon = { highlight = selected, },
        label = { highlight = selected },
        background = { border_color = selected and colors.black or colors.bg2 }
      })
      space_bracket:set({
        background = { border_color = selected and colors.grey or colors.bg2 }
      })
    end)
  
    space:subscribe("mouse.clicked", function()
      sbar.exec("aerospace workspace " .. space_name)
    end)
  
  
  
    space:subscribe("space_windows_change", function()
      sbar.exec("aerospace list-windows --format %{app-name} --workspace " .. space_name, function(windows)
        print(windows)
        local no_app = true
        local icon_line = ""
        for app in windows:gmatch("[^\r\n]+") do
          no_app = false
          local lookup = app_icons[app]
          local icon = ((lookup == nil) and app_icons["default"] or lookup)
          icon_line = icon_line .. " " .. icon
        end
    
        if (no_app) then
          icon_line = " —"
        end
        sbar.animate("tanh", 10, function()
          space:set({ label = icon_line })
        end)
      end)
    end)

    item_order = item_order .. " " .. space.name .. " " .. space_padding.name
  end
  sbar.exec("sketchybar --reorder apple " .. item_order .. " front_app")
end)

```
</details>

---

Created by [@bin101](https://github.com/bin101)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-10841002)
