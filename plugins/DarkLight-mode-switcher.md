# Dark/Light mode switcher
Not really a plugin but just a script to toggle between dark and light mode color schemes for sketchybar. Makes use of the nice animation feature we have.

https://user-images.githubusercontent.com/3230608/235865439-293e2f2e-a815-40c9-b958-bd0443c66c8b.mp4

<details>
<summary> reload_theme.sh </summary>

```bash
#!/bin/zsh

source $HOME/.config/sketchybar/constants.sh
LIGHT_THEME_COLORS=(${LIGHT_THEME_COLORS:l})
DARK_THEME_COLORS=(${DARK_THEME_COLORS:l})

echo "Switching sketchybar to $1 theme.."

for color in $LIGHT_THEME_COLORS; do
  kv=($(echo $color | pcregrep -o1 -o2 --om-separator=" " "(\w*)=(0[xX][0-9a-fA-F]+)"))
  color_name=$kv[1];
  light_c=$kv[2]

  dark_c=$(echo $DARK_THEME_COLORS | pcregrep -o1 "$color_name=(0[xX][0-9a-fA-F]+)")

  sub_map+="s/$([[ $1 = 'dark' ]] && echo "$light_c/$dark_c" || echo "$dark_c/$light_c")/g;"
done

updated_item_properties() {
  sketchybar --query $1 |
    jq -r 'paths as $k | select($k[-1]|tostring | test("color$")) | ($k|join("."))+"="+getpath($k)' |
    sed -e "${sub_map}s/geometry.//g;" | tr ' \n' ' '
}

sketchybar --animate linear 25 --bar $(updated_item_properties bar) &
sketchybar --query bar | jq -r ".items[]" | while read -r item; do
    sketchybar --animate linear 3 --set $item $(updated_item_properties $item) &
done
sketchybar --default label.color=$DEFAULT_LABEL_COLOR icon.color=$DEFAULT_ICON_COLOR
```
</details>

<details>
<summary> Relevant part of constants.sh (Also sourced in sketchybarrc)</summary>

```bash
LIGHT_THEME_COLORS=(
  WHITE_FADED_PLUS='0x55575279'
  WHITE_FADED='0x77575279'
  WHITE='0xff575279'
  BACKGROUND='0xfffaf4ed'
)
DARK_THEME_COLORS=(
  CYAN='0xff88C0D0'
  BLUE='0xff81A1C1'
  MAGENTA='0xffB48EAD'
  ORANGE='0xffffa500'
  GREEN='0xffA3BE8C'
  YELLOW='0xffEBCB8B'
  RED='0xffBF616A'
  WHITE='0xffd8dee9'
  WHITE_FADED='0x55ffffff'
  WHITE_FADED_PLUS='0x11ffffff'
  BACKGROUND='0xff2E3440'
  TRANSPARENT='0x00000000'
)

eval $DARK_THEME_COLORS
grep -q lighttheme ~/.config/kitty/theme.conf && eval $LIGHT_THEME_COLORS

DEFAULT_ICON_COLOR=$WHITE
DEFAULT_LABEL_COLOR=$WHITE

```
</details>

I load dark colors by default, and only have few light colors set so i override based on the current mode of my `kitty`.

The script reads through the color schemes in `constants.sh` and decides which one is the target mode based on the first argument being `light` or `dark`. 
Creates a substitution map for all the overridden colors set in `LIGHT_THEME_COLORS`.
Queries each item in sketchybar for `color$` named properties and converts the nested paths to dot notation ( `{'item': {'background': {'color': '#134'}}}` -> `item.background.color=134`. 
Applies the substitution to and updates Sketchybar with the updated properties.

This probably can be set to trigger on os dark mode change so it applies automatically when you change it. But in my use case i already have a script keybounded to toggle dark/light mode for kitty, vim, os and wallpaper so just included this there.

------------------------

For anyone interested on how I use it:
<details>
<summary> toggle_theme.sh </summary>

```bash
#!/bin/zsh

# Read current theme from kitty and decide on the target mode
grep -q darktheme ~/.config/kitty/theme.conf && target="light" || target="dark"
echo "#${target}theme\ninclude ./${target}.conf" > ~/.config/kitty/theme.conf

# Update theme on running nvim instances
nvr --nostart --serverlist | xargs -I _s_ -n1 nvr --nostart --servername _s_ -cc "lua Theme('$target')"

# Switch wallpaper
cp ~/.config/wp/wp_${target}.png ~/.config/wp/wp.png && killall Dock &

# Send refresh signal to kitty
pkill -USR1 kitty

# Reload sketchybar theme
~/.config/sketchybar/reload_theme.sh $target &

# Change system dark mode
osascript -e "tell app \"System Events\" to tell appearance preferences to set dark mode to $target=dark" &

```
</details>

<details>
<summary> nvim lua function to set theme </summary>

```lua
-- needs to be run at least once on init to load correct theme on startup
function Theme(target)
  -- run_shell is just a util method that captures stdout of shell command
  target = target or (string.match(run_shell('head -1 ~/.config/kitty/theme.conf'), 'darktheme') and 'dark' or 'light')

  schemes = {light='dawnfox', dark='nordfox'}
  target_scheme = schemes[target]
  current_scheme = vim.g.colors_name

  if target_scheme ~= current_scheme then
    vim.cmd('colorscheme '..target_scheme)
    vim.g.colors_name = target_scheme
    print('Changed color scheme to '..target_scheme)
  end
end

```
</details>

<details>
<summary> userChrome.css for Firefox </summary>

```css
/* about:config > toolkit.legacyUserProfileCustomizations.stylesheets */
/* get profile location > Menu > Help > Troubleshooting Information > Profile Directory */
/* mkdir <profile>/chrome */
/* cp .userChrome.css <profile>/chrome */

#sidebar-header { display: none; }
#navigator-toolbox { border: none !important }

/* 'Sideberry settings' > 'Help' > 'Add Preface' = 'on' */
/* 'Sideberry settings' > 'Help' > 'Preface value' = '[Sideberry]' */
/* Add a spring on firefox toolbox edit on left to make space for os close/minimize buttons */
#main-window #TabsToolbar { margin-bottom: 0px !important }
#main-window .toolbar-items { display: block !important }
#main-window #customizableui-special-spring1 { display: none !important }
#main-window[titlepreface*="[Sidebery]"] #TabsToolbar { margin-bottom: -40px !important }
#main-window[titlepreface*="[Sidebery]"] .toolbar-items { display: none !important }
#main-window[titlepreface*="[Sidebery]"] #customizableui-special-spring1 { display: block !important }


/*  Theme */
/* Copied the style of main window (when inspecting firefox broswer) after making a theme on color.firefox.com */
/* Firefox theme should be auto and FF colors plugins should be disabled */
@media (prefers-color-scheme: light) {
  :root {
    --lwt-text-color: rgba(87, 82, 121) !important;
    --lwt-accent-color: rgb(250, 244, 237) !important;
    --arrowpanel-background: rgb(250, 244, 237) !important;
    --panel-disabled-color: rgba(87, 82, 121, 0.5) !important;
    --panel-description-color: rgba(87, 82, 121, 0.7) !important;
    --arrowpanel-color: rgba(87, 82, 121, 1) !important;
    --toolbar-field-background-color: rgb(253, 249, 246) !important;
    --toolbar-field-color: rgb(87, 82, 121) !important;
    --toolbar-field-focus-background-color: rgba(253, 249, 246, 1) !important;
    --toolbar-field-focus-color: rgb(87, 82, 121) !important;
    --toolbar-bgcolor: rgb(250, 244, 237) !important;
    --toolbar-color: rgb(87, 82, 121) !important;
  }
}

@media (prefers-color-scheme: dark) {
 :root {
    --lwt-text-color: rgba(215, 226, 239) !important;
    --lwt-accent-color: rgb(46, 52, 64) !important;
    --arrowpanel-background: rgb(46, 52, 64) !important;
    --panel-disabled-color: rgba(215, 226, 239, 0.5) !important;
    --panel-description-color: rgba(215, 226, 239, 0.7) !important;
    --arrowpanel-color: rgba(215, 226, 239, 1) !important;
    --toolbar-field-background-color: rgb(46, 52, 64) !important;
    --toolbar-field-color: rgb(255, 255, 255) !important;
    --toolbar-field-focus-background-color: rgba(46, 52, 64, 1) !important;
    --toolbar-field-focus-color: rgb(255, 255, 255) !important;
    --toolbar-bgcolor: rgb(46, 52, 64) !important;
    --toolbar-color: rgb(255, 255, 255) !important;
 }
}

/*  Sideberry theme */
/*  V - should be pasted in 'Sideberry settings' > Styles - V */
/**/
/* @media (prefers-color-scheme: dark) { */
/*  #root { */
/*     --bg: rgb(46, 52, 64) !important; */
/*     --tabs-activated-bg: rgb(29, 33, 41) !important; */
/*  } */
/* } */
/**/
/* @media (prefers-color-scheme: light) { */
/*  #root { */
/*     --bg: rgb(250, 244, 237) !important; */
/*     --tabs-activated-bg: rgb(212, 207, 201) !important; */
/*     --pinned-dock-overlay-bg: rgba(250, 244, 237,.5) !important */
/*  } */
/* } */

```
</details>


---

Created by [@zcag](https://github.com/zcag)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-5788874)
