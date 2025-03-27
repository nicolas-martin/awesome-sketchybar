# Collapsible Menu Bar Items

This was pain to get to work properly, particularly with the animation. Hopefully it helps someone else!

## Preview

https://github.com/user-attachments/assets/bbc12ad8-e424-45db-a48a-27c3827d92d3

## Guide

Dependencies:
- https://github.com/FelixKratz/SbarLua
- [Menus helper](https://github.com/FelixKratz/dotfiles/tree/e27673f07ff41eb6a4816daabb79b0a5e837a105/.config/sketchybar/helpers/menus), the executable is expected in `~/.config/sketchybar/helpers/menus/bin/menus`

<details>
<summary>~/.config/sketchybar/items/menus.lua</summary>

```lua
local settings = require("settings")

-- Create a menu trigger item
local menu_item = sbar.add("item", "menu_trigger", {
    drawing = true,
    updates = true,
    icon = {
        font = {
            size = 14.0
        },
        padding_left = 12,
        padding_right = 12,
        string = "≡",
    },
    label = { drawing = false }
})

menu_item:subscribe("mouse.clicked", function(env)
    sbar.trigger("swap_menus_and_spaces")
end)

-- Maximum number of menu items to display
local max_items = 15
local menu_items = {}

-- Create the menu items that will appear inline
for i = 1, max_items, 1 do
    local menu = sbar.add("item", "menu." .. i, {
        position = "left", -- Position them on the left of the bar
        drawing = false,   -- Hidden by default
        icon = { drawing = false },
        label = {
            font = {
                style = settings.font.style_map["Semibold"]
            },
            padding_left = 8,
            padding_right = 8,
        },
        click_script = "$CONFIG_DIR/helpers/menus/bin/menus -s " .. i,
    })
    menu_items[i] = menu
end

-- Menu watcher to monitor app changes
local menu_watcher = sbar.add("item", {
    drawing = false,
    updates = false,
})

-- Menu state variable
local menu_visible = false

-- Function to update menu contents
local function update_menus()
    sbar.exec("$CONFIG_DIR/helpers/menus/bin/menus -l", function(menus)
        -- Reset all menu items
        for i = 1, max_items do
            menu_items[i]:set({ drawing = false, width = 0 })
        end

        -- Update with new menu items
        local id = 1
        for menu in string.gmatch(menus, '[^\r\n]+') do
            if id <= max_items then
                menu_items[id]:set({
                    label = {
                        string = menu,
                    },
                    drawing = menu_visible,
                    width = menu_visible and "dynamic" or 0
                })
            else
                break
            end
            id = id + 1
        end
    end)
end

-- Function to toggle the menu
local function toggle_menu()
    -- Toggle the menu state
    menu_visible = not menu_visible

    if menu_visible then
        -- Show menu items with animation
        menu_watcher:set({ updates = true })

        -- Prepare menu items but keep them hidden until animation starts
        update_menus()

        -- Initialize items with drawing=false and width=0
        for i = 1, max_items do
            local query = menu_items[i]:query()
            local has_content = query.label.string ~= ""

            if has_content then
                -- Make sure items aren't visible at 0 width
                menu_items[i]:set({
                    drawing = false,
                    width = 0,
                    label = {
                        drawing = false
                    }
                })
            end
        end

        -- First make them drawing=true but with label still hidden
        for i = 1, max_items do
            local query = menu_items[i]:query()
            local has_content = query.label.string ~= ""

            if has_content then
                menu_items[i]:set({ drawing = true })
            end
        end

        -- Animate the expansion
        sbar.animate("tanh", 30, function()
            for i = 1, max_items do
                local query = menu_items[i]:query()
                local is_drawing = query.geometry.drawing == "on"

                if is_drawing then
                    -- First set the width
                    menu_items[i]:set({ width = "dynamic" })
                    -- Then make label visible
                    menu_items[i]:set({
                        label = {
                            drawing = true
                        }
                    })
                end
            end
        end)
    else
        update_menus()
        -- Hide menu items with animation
        sbar.animate("tanh", 30, function()
            for i = 1, max_items do
                menu_items[i]:set({ width = 0 })
            end
        end, function()
            for i = 1, max_items do
                menu_items[i]:set({ drawing = false })
            end
            menu_watcher:set({ updates = false })
        end)
    end
end

-- Click to toggle menu
menu_item:subscribe("mouse.clicked", function(env)
    toggle_menu()
end)

-- Subscribe to front app changes
menu_watcher:subscribe("front_app_switched", function(env)
    update_menus()
end)

-- Initial update
update_menus()

return menu_watcher
```
</details>

---

Created by [@wolveix](https://github.com/wolveix)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-12312842)
