# Search, Get, and Set Wallpaper via [unsplash.com](https://unsplash.com)

There are native menu bar solutions that do this, but none of them provide the ability to search for a specific term. Voilà:

![2023-10-14 15 07 24](https://github.com/FelixKratz/SketchyBar/assets/445891/bb2a7ee4-d210-41c7-9e04-722dbf74d70b)
_Edit: after posting I realized how small this is. I'm changing the `QUERY` variable from "red" to "blue" then clicking the item in SketchyBar_

## Caution

- There are reports that macos Sonoma's wallpaper system is broken. I banged my head against the wall building this. Walked away, came back the next morning, and time did the trick. Here be dragons.
- This only changes all _displays_, but not spaces. If anyone knows how this can be done, please share!
- The `TIMESTAMP` variable appears necessary as macos caches desktop images by filename (?)

## Instructions

- Set up a `.secrets` file in `$HOME/.config`.
  - Use the `.secrets` example below as a template - **do not commit your API Key to dotfiles**
- Set up Unsplash API Access/Account
  - [applications page](https://unsplash.com/oauth/applications)
  - [register one here](https://unsplash.com/oauth/applications/new)
    - free but **limited to 50 requests / hour**
- Add `ACCESS_KEY` to `.secrets` with the "Access Key" from the Unsplash API, or another preferred local secrets manager
- Set up writeable directory to store saved images
- Change `WALLPAPER_PATH` in `plugins/wallpaper.sh` to the writeable directory's path
- Remember to double and triple check permissions on directories and files

## To Do

- [ ] create a popup with multiple returned images and the ability to choose
- [ ] create a "search again" button in the popup
- [ ] create a "change search term" button, macos dialog box, and script to update the `QUERY` variable without opening `plugins/wallpaper.sh`

Please share your thoughts and ideas! Cheers :)

<details>
  <summary>$HOME/.secrets</summary>
  
```bash
#!/bin/bash

ACCESS_KEY="ABCDEFGhijKlMNOp"
```
</details>

<details>
  <summary>items/wallpaper.sh</summary>
  
```bash
#!/bin/bash

# `nf-md-wall`
ICON=󰟾

wallpaper=(
  drawing=on
  click_script="$PLUGIN_DIR/wallpaper.sh"
  background.padding_left=3
  background.padding_right=3
  icon=$ICON
  icon.color=$WHITE
  icon.padding_left=3
  icon.padding_right=3
  label.drawing=off
  # associated_display=1
)

sketchybar --add item wallpaper right \
  --set wallpaper "${wallpaper[@]}"
```
</details>

<details>
  <summary>plugins/wallpaper.sh</summary>
  
```bash
#!/bin/bash

source "$HOME/.config/sketchybar/colors.sh"
source "$HOME/.config/.secrets"

# Update
QUERY="blue"
ORIENTATION="landscape"
TIMESTAMP=$(echo '('$(date +"%s.%N") ' * 10)/1' | bc)
WALLPAPER_PATH="$HOME/.config/.sketchyrw/wallpaper"

# TODO: Create popup with multiple options to choose from, search again, or change query via macos dialog

URL=$(
  curl --location "https://api.unsplash.com/photos/random?query=${QUERY}&orientation=${ORIENTATION}" \
    --header "Authorization: Client-ID ${ACCESS_KEY}" | jq -r '.urls.full'
)
# Note: change the `jq` query to `.urls.raw` for larger displays

wget $URL -O "${WALLPAPER_PATH}/wallpaper_${TIMESTAMP}.jpg"
WALLPAPER="${WALLPAPER_PATH}/wallpaper_${TIMESTAMP}.jpg"

# TODO: Set wallpaper on all spaces via yet-to be determined Mission Control API
osascript -e "tell application \"System Events\" to tell every desktop to set picture to \"$WALLPAPER\" as POSIX file"
```
</details>

<details>
  <summary>sketchybarrc</summary>
  
```bash
#!/bin/bash

source "$CONFIG_DIR/colors.sh" # Loads all defined colors
source "$CONFIG_DIR/icons.sh"  # Loads all defined icons

ITEM_DIR="$CONFIG_DIR/items"     # Directory where the items are configured
PLUGIN_DIR="$CONFIG_DIR/plugins" # Directory where all the plugin scripts are stored

FONT="Hack Nerd Font" # Needs to have Regular, Bold, Semibold, Heavy and Black variants
PADDINGS=3            # All paddings use this value (icon, label, background)

# Setting up and starting the helper process
HELPER=git.felix.helper
killall helper
(cd $CONFIG_DIR/helper && make)
$CONFIG_DIR/helper/helper $HELPER >/dev/null 2>&1 &

# Unload the macOS on screen indicator overlay for volume change
launchctl unload -F /System/Library/LaunchAgents/com.apple.OSDUIHelper.plist >/dev/null 2>&1 &

# Setting up the general bar appearance of the bar
bar=(
  height=50
  color=$BAR_COLOR
  border_width=0
  border_color=$BAR_BORDER_COLOR
  corner_radius=9
  shadow=off
  position=top
  sticky=on
  padding_right=8
  padding_left=8
  y_offset=1
  margin=6
  topmost=window
)

sketchybar --bar "${bar[@]}"

# Setting up default values
defaults=(
  updates=when_shown
  icon.font="$FONT:Bold:14.0"
  icon.color=$ICON_COLOR
  icon.padding_left=$PADDINGS
  icon.padding_right=$PADDINGS
  label.font="$FONT:Semibold:13.0"
  label.color=$LABEL_COLOR
  label.padding_left=$PADDINGS
  label.padding_right=$PADDINGS
  padding_right=$PADDINGS
  padding_left=$PADDINGS
  background.height=26
  background.corner_radius=50
  background.border_width=1
  popup.y_offset=0
  popup.background.border_width=1
  popup.background.corner_radius=9
  popup.background.border_color=$BACKGROUND_2
  popup.background.color=$POPUP_BACKGROUND_COLOR
  popup.blur_radius=20
  popup.background.shadow.drawing=on
)

sketchybar --default "${defaults[@]}"

# Left
# source "$ITEM_DIR/front_app.sh" moved inside of apple.sh
source "$ITEM_DIR/apple.sh"
source "$ITEM_DIR/spaces.sh"
source "$ITEM_DIR/yabai.sh"

# Center
source "$ITEM_DIR/music.sh"

# Right
source "$ITEM_DIR/battery.sh"
source "$ITEM_DIR/calendar.sh"

# Wallpaper Item Here
source "$ITEM_DIR/wallpaper.sh"

# Forcing all item scripts to run (never do this outside of sketchybarrc)
sketchybar --update

echo "sketchybar configuation loaded.."
```
</details>

P.S. @FelixKratz thank you for the lovely project _and_ publishing your dotfiles :)

---

Created by [@dgrebb](https://github.com/dgrebb)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-7282317)
