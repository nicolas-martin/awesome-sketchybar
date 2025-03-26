# Aerospace Workspaces

Every other implementation of Aerospace workspaces within Sketchybar had weird issues in my experience, either they wouldn't reflect changes without switching workspaces, or wouldn't show empty workspaces after they contained applications.

The one thing to note is that it handles updates by listening for workspaces changes set by Aerospace, as well as app changes. So if you move an app from workspace to another **and there's another app within your current workspace**, it'll immediately reflect the change as your focus will be shifted to your other app. If there isn't another app, the change will be reflected when you change workspace, or switch app focus.

If you struggle to get this to work, please take a look at my complete Aerospace + Sketchybar config (the zip file appears empty, as the files are dot files): [sketchybar-aerospace.zip](https://github.com/user-attachments/files/19170385/sketchybar-aerospace.zip)


Extract this to your home directory, then run `aerospace reload-config && sketchybar --reload`.

## Preview

https://github.com/user-attachments/assets/d96ad45b-e87d-4850-ae82-eef8d83bddbd

## Guide

Dependencies:
- Lua (`brew install lua`)
- `~/.config/sketchybar/helpers/app_icons.lua`
- In `~/.config/sketchybar/default.lua`, make sure you've specified `updates = "on"`, otherwise the spaces won't update correctly

<details>
<summary>~/.config/sketchybar/items/workspaces.lua</summary>

```lua
local colors = require("colors")
local settings = require("settings")
local app_icons = require("helpers.app_icons")

local query_workspaces = "aerospace list-workspaces --all --format '%{workspace}%{monitor-appkit-nsscreen-screens-id}' --json"

-- Root is used to handle event subscriptions
local root = sbar.add("item", { drawing = false, })
local workspaces = {}

local function withWindows(f)
    local open_windows = {}
    -- Include the window ID in the query so we can track unique windows
    local get_windows = "aerospace list-windows --monitor all --format '%{workspace}%{app-name}%{window-id}' --json"
    local query_visible_workspaces =
    "aerospace list-workspaces --visible --monitor all --format '%{workspace}%{monitor-appkit-nsscreen-screens-id}' --json"
    local get_focus_workspaces = "aerospace list-workspaces --focused"
    sbar.exec(get_windows, function(workspace_and_windows)
        -- Use a set to track unique window IDs
        local processed_windows = {}

        for _, entry in ipairs(workspace_and_windows) do
            local workspace_index = entry.workspace
            local app = entry["app-name"]
            local window_id = entry["window-id"]

            -- Only process each window ID once
            if not processed_windows[window_id] then
                processed_windows[window_id] = true

                if open_windows[workspace_index] == nil then
                    open_windows[workspace_index] = {}
                end

                -- Check if this app is already in the list for this workspace
                local app_exists = false
                for _, existing_app in ipairs(open_windows[workspace_index]) do
                    if existing_app == app then
                        app_exists = true
                        break
                    end
                end

                -- Only add the app if it's not already in the list
                if not app_exists then
                    table.insert(open_windows[workspace_index], app)
                end
            end
        end

        sbar.exec(get_focus_workspaces, function(focused_workspaces)
            sbar.exec(query_visible_workspaces, function(visible_workspaces)
                local args = {
                    open_windows = open_windows,
                    focused_workspaces = focused_workspaces,
                    visible_workspaces = visible_workspaces
                }
                f(args)
            end)
        end)
    end)
end

local function updateWindow(workspace_index, args)
    local open_windows = args.open_windows[workspace_index]
    local focused_workspaces = args.focused_workspaces
    local visible_workspaces = args.visible_workspaces

    if open_windows == nil then
        open_windows = {}
    end

    local icon_line = ""
    local no_app = true
    for i, open_window in ipairs(open_windows) do
        no_app = false
        local app = open_window
        local lookup = app_icons[app]
        local icon = ((lookup == nil) and app_icons["Default"] or lookup)
        icon_line = icon_line .. " " .. icon
    end

    sbar.animate("tanh", 10, function()
        for i, visible_workspace in ipairs(visible_workspaces) do
            if no_app and workspace_index == visible_workspace["workspace"] then
                local monitor_id = visible_workspace["monitor-appkit-nsscreen-screens-id"]
                icon_line = " —"
                workspaces[workspace_index]:set({
                    drawing = true,
                    label = { string = icon_line },
                    display = monitor_id,
                })
                return
            end
        end
        if no_app and workspace_index ~= focused_workspaces then
            workspaces[workspace_index]:set({
                drawing = false,
            })
            return
        end
        if no_app and workspace_index == focused_workspaces then
            icon_line = " —"
            workspaces[workspace_index]:set({
                drawing = true,
                label = { string = icon_line },
            })
        end

        workspaces[workspace_index]:set({
            drawing = true,
            label = { string = icon_line },
        })
    end)
end

local function updateWindows()
    withWindows(function(args)
        for workspace_index, _ in pairs(workspaces) do
            updateWindow(workspace_index, args)
        end
    end)
end

local function updateWorkspaceMonitor()
    local workspace_monitor = {}
    sbar.exec(query_workspaces, function(workspaces_and_monitors)
        for _, entry in ipairs(workspaces_and_monitors) do
            local space_index = entry.workspace
            local monitor_id = math.floor(entry["monitor-appkit-nsscreen-screens-id"])
            workspace_monitor[space_index] = monitor_id
        end
        for workspace_index, _ in pairs(workspaces) do
            workspaces[workspace_index]:set({
                display = workspace_monitor[workspace_index],
            })
        end
    end)
end

sbar.exec(query_workspaces, function(workspaces_and_monitors)
    for _, entry in ipairs(workspaces_and_monitors) do
        local workspace_index = entry.workspace

        local workspace = sbar.add("item", {
            background = {
                color = colors.bg1,
                drawing = true,
            },
            click_script = "aerospace workspace " .. workspace_index,
            drawing = false, -- Hide all items at first
            icon = {
                color = colors.with_alpha(colors.white, 0.3),
                drawing = true,
                font = { family = settings.font.numbers },
                highlight_color = colors.white,
                padding_left = 5,
                padding_right = 4,
                string = workspace_index
            },
            label = {
                color = colors.with_alpha(colors.white, 0.3),
                drawing = true,
                font = "sketchybar-app-font:Regular:16.0",
                highlight_color = colors.white,
                padding_left = 2,
                padding_right = 12,
                y_offset = -1,
            },
        })

        workspaces[workspace_index] = workspace

        workspace:subscribe("aerospace_workspace_change", function(env)
            local focused_workspace = env.FOCUSED_WORKSPACE
            local is_focused = focused_workspace == workspace_index

            sbar.animate("tanh", 10, function()
                workspace:set({
                    icon = { highlight = is_focused },
                    label = { highlight = is_focused },
                    blur_radius = 30,
                })
            end)
        end)
    end

    -- Initial setup
    updateWindows()
    updateWorkspaceMonitor()

    -- Subscribe to window creation/destruction events
    root:subscribe("aerospace_workspace_change", function()
        updateWindows()
    end)

    -- Subscribe to front app changes too
    root:subscribe("front_app_switched", function()
        updateWindows()
    end)

    root:subscribe("display_change", function()
        updateWorkspaceMonitor()
        updateWindows()
    end)

    sbar.exec("aerospace list-workspaces --focused", function(focused_workspace)
        local focused_workspace = focused_workspace:match("^%s*(.-)%s*$")
        workspaces[focused_workspace]:set({
            icon = { highlight = true },
            label = { highlight = true },
        })
    end)
end)
```
</details>

---

Created by [@wolveix](https://github.com/wolveix)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-12317412)
