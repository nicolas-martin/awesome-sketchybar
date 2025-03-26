# Media Info Slider

Thanks for the fantastic application, and for SbarLua!

![media-slider](https://github.com/user-attachments/assets/9d0b07e5-b535-44cb-a746-5f98c5f29096)

This is based on https://github.com/FelixKratz/SbarLua/blob/main/example/items/media.lua. On mouseover, it expands to display your playing media from whitelisted apps. Click toggles "Play/Pause".

## Preparation
Launch the `Shortcuts` app. Create a shortcut called "playpause" with "Play/Pause" from the "Media" category. This shortcut will be used by the item for playback control.

*If anyone has suggestions for a better way to implement "Play/Pause" please let me know!*

## SbarLua module

Adjust as needed with your colors, icons and fonts.

```lua
local colors = require 'colors'
local app_icons = require 'app_icons'

local whitelist = {
	['Spotify'] = true,
	['Music'] = true,
	['Podcasts'] = true,
	['VLC'] = true,
	['IINA'] = true,
	['Arc'] = true
};

local media = sbar.add('item', 'media', {
	icon = {
		font = 'sketchybar-app-font:Regular:12.0',
		color = colors.media,
	},
	label = {
		font = 'IosevkaTerm Nerd Font:Regular:14.0',
		width = 20,
		padding_right = 12,
		color = colors.media,
		background = {
			color = colors.inactive_bg,
			corner_radius = 10,
			height = 24,
		}
	},
	position = 'center',
	updates = true,
	background = {
		color = colors.inactive_bg,
		corner_radius = 10,
		height = 24,
	},
	width = 24,
})

local function animate_media_width(width)
	sbar.animate('tanh', 30.0, function()
		media:set({ label = { width = width } })
	end)
end

media:subscribe('mouse.entered', function()
	local text = media:query().label.value
	animate_media_width(#text * 7) -- adjust depending on font width
end)
media:subscribe('mouse.exited', function()
	animate_media_width(20)
end)
media:subscribe('mouse.clicked', function(env)
	sbar.exec('shortcuts run "playpause"')
end)

media:subscribe('media_change', function(env)
	if whitelist[env.INFO.app] then
		local lookup = app_icons[env.INFO.app]
		local icon = ((lookup == nil) and app_icons['default'] or lookup)

		local playback_icon = ((env.INFO.state == 'playing') and '' or '')

		local artist = (env.INFO.artist ~= "" and env.INFO.artist) or "Unknown Artist"
		local title = (env.INFO.title ~= "" and env.INFO.title) or "Unknown Title"
		local label = playback_icon .. ' ' .. artist .. ': ' .. title

		sbar.animate('tanh', 10, function()
			media:set({
				icon = { string = icon },
				label = label
			})
		end)
	end
end)
```

---

Created by [@joncrangle](https://github.com/joncrangle)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-10424166)
