# Connect/Disconnect GlobalProtect

An orange network icon that when is clicked when the GlobalProtect is switched on/off.

<img width="417" alt="Screenshot 2024-11-05 at 14 28 42" src="https://github.com/user-attachments/assets/4a21d356-fcf9-4603-9825-cc90c1e11c56">


```lua
--globalprotect.lua
local icons = require("icons")
local colors = require("colors")
local settings = require("settings")

local is_on = false

local globalprotect_icon = sbar.add("item", "widgets.globalprotect", {
    position = "right",
    padding_right = 0,
    icon = {
      string = icons.network,
      width = 0,
      align = "left",
      color = colors.orange,
      font = {
        style = settings.font.style_map["Regular"],
        size = 16.0      
      },
      color = colors.orange
    },
    label = {
      width = 30,
      align = "left",
      font = {
        style = settings.font.style_map["Regular"],
        size = 14.0
      },
      color = colors.orange
    },
  })

local globalprotect_bracket = sbar.add("bracket", "widgets.globalprotect.bracket", {
    globalprotect_icon.name
  }, {
    background = { color = colors.bar_color, 
    border_color = colors.orange,
    border_width = colors.border_width
  },
    popup = { align = "center" }
  })
 
sbar.add("item", "widgets.globalprotect.padding", {
position = "right",
width = settings.group_paddings
})


local function runScript()
    os.execute([[
        osascript -e '
            tell application "System Events" to tell process "GlobalProtect"
                click menu bar item 1 of menu bar 2 -- Activates the GlobalProtect "window" in the menubar
                set frontmost to true -- keep window 1 active
                tell window 1
                    -- Click on the connect or disconnect button, depending on if they exist or not
                    if exists (first UI element whose title is "Connect") then
                        tell (first UI element whose title is "Connect") to if exists then click
                    else
                        tell (first UI element whose title is "Disconnect") to if exists then click
                    end if
                end tell
                click menu bar item 1 of menu bar 2 -- This will close the GlobalProtect "window" after clicking Connect/Disconnect. This is optional.
            end tell
        '
    ]])
end

local function switchOff()
    runScript()
    is_on = false
    globalprotect_icon:set({ 
        icon = { color = colors.orange },
        background = { border_color = colors.orange }
    })
    globalprotect_bracket:set({
        background = { border_color = colors.orange }
    })
end

local function switchOn()
    runScript()
    is_on = true
    globalprotect_icon:set({ 
        icon = { color = colors.red }        
    })
    globalprotect_bracket:set({
        background = { border_color = colors.red }
    })
end

local function toggleGlobalProtectConnection(env)
    if is_on == true then
        switchOff()
    else
        switchOn()
    end
end


globalprotect_icon:subscribe("mouse.clicked", toggleGlobalProtectConnection)
```

Credits to the creator of the osascript : https://gist.github.com/kaleksandrov/3cfee92845a403da995e7e44ba771183?permalink_comment_id=4416917#gistcomment-4416917



---

Created by [@noeliajimenezg](https://github.com/noeliajimenezg)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-11154914)
