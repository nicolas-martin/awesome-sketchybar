# Updated Window Title For Lua
 

https://github.com/FelixKratz/SketchyBar/assets/159923978/d406ee1b-36b5-4586-ae91-c39ffa1bd5cb


The meat and bones of this are mostly the same as the first shell-based version that typkrft posted at the beginning of this thread, however there were a few changes that I made.
* Instead of using jq to parse the information, we can query the Lua table automatically made from the Yabai json output
* The Yabai command has been moved to a standalone shell script.

I found that if the window query command is ran when focused on a space without any windows, the command would return "could not retrieve window details.", without passing anything to sketchybar. Because of this the title would stay as the title of the last focused window, instead of reflecting that Finder was now focused. With the command ran through an external script, when no windows are present we will get an "empty"  response back, so that the title can be set to "Finder" 

<details>
<summary>yabairc</summary>

``` sh
# S K E T C H Y B A R  E V E N T S
yabai -m signal --add event=window_focused action="sketchybar --trigger window_focus &> /dev/null"
yabai -m signal --add event=window_title_changed action="sketchybar --trigger title_change &> /dev/null"
```
</details>

<details>
<summary>query_window.sh</summary>
This should be saved to a place that sketchybar can find. I have it saved in my `~/.scripts/` directory.

``` sh
#!/bin/sh

# Query yabai for window info
yabai_output=$(yabai -m query --windows --window)

# Check if the output is empty
if [ -z "$yabai_output" ]; then
  # Output is empty, print "empty" to indicate no windows
  echo "empty"
else
  # Output is not empty, print the yabai output
  echo "$yabai_output"
fi
```
</details>

<details>
<summary>front_app.lua</summary>
The title is limited to 50 characters as before, and will be followed by ellipses if it is any longer. I also added the app icon with a short grow/shrink animation when it changes, like how it was with felix' old shell-based config. I've had to use sleep to give the first part of the animation enough time to complete, as if both animations were executed one after another they wouldn't do anything. Not too sure if that's working as intended or if it should be handled in a different manner.

``` lua
local colors = require("colors")
local settings = require("settings")

-- Events that get pushed by yabai
sbar.add("event", "window_focus")
sbar.add("event", "title_change")

local front_app = sbar.add("item", "front_app", {
  position = "left",
  display = "active",
  icon = {
    background = {
      drawing = true,
      image = {
        border_width = 1,
        border_color = colors.bg1,
      }
    },
  },
  label = {
    font = {
      style = settings.font.style_map["Black"],
      size = 12.0,
    },
  },
  updates = true,
})

local function set_window_title()
  -- Offloading the "yabai -m query --windows --window" script to an external shell script so that we can determine whether the space has no windows
  sbar.exec("~/.scripts/sketchybar/query_window.sh", function(result)
    if result ~= "empty" and type(result) == "table" and result.title then
      local window_title = result.title
      if #window_title > 50 then
        window_title = window_title:sub(1, 50) .. "..."
      end
      front_app:set({ label = { string = window_title } })
    else
      -- Set title to Finder, as empty spaces will not return a window title
      front_app:set({ label = { string = "Finder" } })
    end
  end)
end

-- Animate app icon back to 1.0
local function end_bounce_animation()
  sbar.animate("tanh", 15, function()
    front_app:set({
      icon = {
        background = {
          image = { scale = 1.0 },
	}
      }
    })
  end)
end

-- Make app icon slightly bigger before returning back to regular size
local function start_bounce_animation()
  sbar.animate("tanh", 15, function()
    front_app:set({
      icon = {
        background = {
          image = { scale = 1.2 },
	}
      }
    })
  end)
  -- Short delay so that full animation can occur
  sbar.exec("sleep 0.25 && echo 'finishing bounce'", end_bounce_animation) 
end

front_app:subscribe("front_app_switched", function(env)
  front_app:set({
    icon = { background = { image = "app." .. env.INFO } }
  })
  set_window_title()
  start_bounce_animation()
end)

front_app:subscribe("space_change", function()
  set_window_title()
  start_bounce_animation()
end)

front_app:subscribe("window_focus", function()
  set_window_title()
  start_bounce_animation()
end)

front_app:subscribe("title_change", function()
  set_window_title()
end)

front_app:subscribe("mouse.clicked", function(env)
  sbar.trigger("swap_menus_and_spaces")
end)
```
</details>


---

Created by [@matthewben-net](https://github.com/matthewben-net)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-9599450)
