# Dark/Light theme switcher

https://user-images.githubusercontent.com/3230608/235452989-6bb9e348-8a07-45e5-8dca-4d77ac623f01.mp4

It replaces all the colors on the fly without needing to restart sketchybar, makes use of the nice animation feature as well..

<details>
  <summary>reload_theme.sh</summary>

```bash
#!/bin/zsh

source $HOME/.config/sketchybar/constants.sh
LIGHT_THEME_COLORS=(${LIGHT_THEME_COLORS:l})
DARK_THEME_COLORS=(${DARK_THEME_COLORS:l})

echo "Switching sketchybar to $1 theme.."

for color in $LIGHT_THEME_COLORS; do
  key_value=($(echo $color | pcregrep -o1 -o2 --om-separator=" " "(\w*)=(0[xX][0-9a-fA-F]+)"))

  light_color=${key_value[2]}
  dark_color=$(echo $DARK_THEME_COLORS | pcregrep -o1 "${key_value[1]}=(0[xX][0-9a-fA-F]+)")

  [[ $1 = 'light' ]] && { source_colors+=($dark_color ); sub_map+=("s/$dark_color/$light_color/g;") } \
                     || { source_colors+=($light_color); sub_map+=("s/$light_color/$dark_color/g;") }
done

updated_item_properties() {
  sketchybar --query $1 |
    jq -r 'leaf_paths as $path | ($path | join(".")) + "=" + (getpath($path)|tostring) | select(contains("color"))' |
    grep -E "$(echo $source_colors | tr ' ' '|')" |
    sed -e "${sub_map}s/geometry.//g;" |
    tr ' \n' ' '
}

sketchybar --animate linear 10 --bar $(updated_item_properties bar) &
sketchybar --query bar |
  jq -r ".items[]" |
  while read -r item_name; do
    sketchybar --animate linear 10 --set $item_name $(updated_item_properties $item_name) &
  done

```
</details>

The script goes through the `LIGHT_THEME_COLORS` from `constants.sh` to get a list of 'themed colors' since have only couple overriden colors in light mode, and creates a replacement map for the target theme. 

`updated_item_properties` queries sketchybar and converts all the nested properties into the dotted format that `--set` uses (i.e. `label: {background: {border_color: x}} -> label.background.border_color=x`),  filters by the keys that have the string `color` and if the actual color is in our light colors list, then replaces all the colors using the sub_map.

<details>
  <summary>relevant part of my `constants.sh`</summary>

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

```
</details>

This probably can be hooked to the system dark mode change event to automatically switch, but i'm already using a single script to toggle light/dark mode for my wallpaper, os, kitty terminal, and vim. For anyone interested:


<details>
  <summary>toggle_theme.sh</summary>

```bash
#!/bin/zsh

# vim is triggered by /theme.conf
grep -q darktheme ~/.config/kitty/theme.conf && target="light" || target="dark"

# Set kitty theme and send refresh signal
echo "#${target}theme\ninclude ./${target}.conf" > ~/.config/kitty/theme.conf && pkill -USR1 kitty

# Switch wallpaper
cp ~/.config/wp/wp_${target}.png ~/.config/wp/wp.png && killall Dock &

# Change system dark mode
osascript -e "tell app \"System Events\" to tell appearance preferences to set dark mode to $target=dark" &

# Reload sketchybar theme
~/.config/sketchybar/reload_theme.sh $target &

```
</details>


---

Created by [@zcag](https://github.com/zcag)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-5771466)
