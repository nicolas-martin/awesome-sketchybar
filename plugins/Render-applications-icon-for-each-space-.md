# Render applications icon for each space with Nerd Font

<img width="455" alt="activity" src="https://github.com/FelixKratz/SketchyBar/assets/13052949/e08a8461-8ee1-43e1-b3d4-92569b62b484">

<details><summary>colors.sh</summary>

```shell
#!/bin/sh

# Gruvbox Dark (Hard) Color Palette

export COLOR_BACKGROUND=0xe01d2021
export COLOR_FOREGROUND=0xe0fbf1c7

export COLOR_ACCENT=0xe0d65d0e
export COLOR_ACCENT_BRIGHT=0xe0fe8019

export COLOR_BLACK=0xe0282828
export COLOR_RED=0xe0cc241d
export COLOR_GREEN=0xe098971a
export COLOR_YELLOW=0xe0d79921
export COLOR_BLUE=0xe0458588
export COLOR_MAGENTA=0xe0b16286
export COLOR_CYAN=0xe0689d6a
export COLOR_WHITE=0xe0a89984

export COLOR_BLACK_BRIGHT=0xe0928374
export COLOR_RED_BRIGHT=0xe0fb4934
export COLOR_GREEN_BRIGHT=0xe0b8bb26
export COLOR_YELLOW_BRIGHT=0xe0fabd2f
export COLOR_BLUE_BRIGHT=0xe083a598
export COLOR_MAGENTA_BRIGHT=0xe0d3869b
export COLOR_CYAN_BRIGHT=0xe08ec07c
export COLOR_WHITE_BRIGHT=0xe0ebdbb2
```

</details>

<details><summary>icons.sh</summary>

```shell
#!/bin/sh

# Material Design Icons

export ICON_CMD=󰘳
export ICON_COG=󰒓 # system settings, system information, tinkertool
export ICON_CHART=󱕍 # activity monitor, btop
export ICON_LOCK=󰌾

export ICONS_SPACE=(󰎤 󰎧 󰎪 󰎭 󰎱 󰎳 󰎶)

export ICON_APP=󰣆 # fallback app
export ICON_TERM=󰆍 # fallback terminal app, terminal, warp, iterm2
export ICON_PACKAGE=󰏓 # brew
export ICON_DEV=󰅨 # nvim, xcode, vscode
export ICON_FILE=󰉋 # ranger, finder
export ICON_GIT=󰊢 # lazygit
export ICON_LIST=󱃔 # taskwarrior, taskwarrior-tui, reminders, onenote
export ICON_SCREENSAVOR=󱄄 # unimatrix, pipe.sh

export ICON_WEATHER=󰖕 # weather
export ICON_MAIL=󰇮 # mail, outlook
export ICON_CALC=󰪚 # calculator, numi
export ICON_MAP=󰆋 # maps, find my
export ICON_MICROPHONE=󰍬 # voice memos
export ICON_CHAT=󰍩 # messages, slack, teams, discord, telegram
export ICON_VIDEOCHAT=󰍫 # facetime, zoom, webex
export ICON_NOTE=󱞎 # notes, textedit, stickies, word, bat
export ICON_CAMERA=󰄀 # photo booth
export ICON_WEB=󰇧 # safari, beam, duckduckgo, arc, edge, chrome, firefox
export ICON_HOMEAUTOMATION=󱉑 # home
export ICON_MUSIC=󰎄 # music, spotify
export ICON_PODCAST=󰦔 # podcasts
export ICON_PLAY=󱉺 # tv, quicktime, vlc
export ICON_BOOK=󰂿 # books
export ICON_BOOKINFO=󱁯 # font book, dictionary
export ICON_PREVIEW=󰋲 # screenshot, preview
export ICON_PASSKEY=󰷡 # 1password
export ICON_DOWNLOAD=󱑢 # progressive downloader, transmission
export ICON_CAST=󱒃 # airflow
export ICON_TABLE=󰓫 # excel
export ICON_PRESENT=󰈩 # powerpoint
export ICON_CLOUD=󰅧 # onedrive
export ICON_PEN=󰏬 # curve
export ICON_REMOTEDESKTOP=󰢹 # vmware, utm

export ICON_CLOCK=󰥔 # clock, timewarrior, tty-clock
export ICON_CALENDAR=󰃭 # calendar

export ICON_WIFI=󰖩
export ICON_WIFI_OFF=󰖪
export ICON_VPN=󰦝 # vpn, nordvpn

export ICONS_VOLUME=(󰸈 󰕿 󰖀 󰕾)

export ICONS_BATTERY=(󰂎 󰁺 󰁻 󰁼 󰁽 󰁾 󰁿 󰂀 󰂁 󰂂 󰁹)
export ICONS_BATTERY_CHARGING=(󰢟 󰢜 󰂆 󰂇 󰂈 󰢝 󰂉 󰢞 󰂊 󰂋 󰂅)

export ICON_SWAP=󰁯
export ICON_RAM=󰓅
export ICON_DISK=󰋊 # disk utility
export ICON_CPU=󰘚
```

</details>

<details><summary>yabairc</summary>

```shell
# add these lines to the bottom of your yabairc file
# they're needed to trigger a custom event in sketchybar that we will later define in sketchybarrc

yabai -m signal --add event=window_created action="sketchybar -m --trigger window_change &> /dev/null"
yabai -m signal --add event=window_destroyed action="sketchybar -m --trigger window_change &> /dev/null"
```

</details>

<details><summary>sketchybarrc</summary>

```shell
source "$HOME/.config/colors.sh"
source "$HOME/.config/icons.sh"

# Defaults
sketchybar --default padding_left=8                                    \
                     padding_right=8                                   \
                                                                       \
                     background.border_color=$COLOR_YELLOW             \
                     background.border_width=2                         \
                     background.height=40                              \
                     background.corner_radius=12                       \
                                                                       \
                     icon.color=$COLOR_YELLOW                          \
                     icon.highlight_color=$COLOR_BACKGROUND            \
                     icon.padding_left=6                               \
                     icon.padding_right=2                              \
                     icon.font="CaskaydiaCove Nerd Font:Regular:16.0"  \
                                                                       \
                     label.color=$COLOR_YELLOW                         \
                     label.highlight_color=$COLOR_BACKGROUND           \
                     label.padding_left=2                              \
                     label.padding_right=6                             \
                     label.font="CaskaydiaCove Nerd Font:Regular:12.0"

# Register custom event - this will be use by sketchybar's space items as well as app_space.sh
sketchybar --add event window_change

# Space items
COLORS_SPACE=($COLOR_YELLOW $COLOR_CYAN $COLOR_MAGENTA $COLOR_WHITE $COLOR_BLUE $COLOR_RED $COLOR_GREEN)
LENGTH=${#ICONS_SPACE[@]}

for i in "${!ICONS_SPACE[@]}"
do
  sid=$(($i+1))
  PAD_LEFT=2
  PAD_RIGHT=2
  if [[ $i == 0 ]]; then
    PAD_LEFT=8
  elif [[ $i == $(($LENGTH-1)) ]]; then
    PAD_RIGHT=8
  fi
  sketchybar --add space space.$sid left                                       \
             --set       space.$sid script="$PLUGIN_DIR/app_space.sh"          \
                                    associated_space=$sid                      \
                                    padding_left=$PAD_LEFT                     \
                                    padding_right=$PAD_RIGHT                   \
                                    background.color=${COLORS_SPACE[i]}        \
                                    background.border_width=0                  \
                                    background.corner_radius=6                 \
                                    background.height=24                       \
                                    icon=${ICONS_SPACE[i]}                     \
                                    icon.color=${COLORS_SPACE[i]}              \
                                    label="_"                                  \
                                    label.color=${COLORS_SPACE[i]}             \
             --subscribe space.$sid front_app_switched window_change
done

# Space bracket
sketchybar --add bracket spaces '/space\..*/'                      \
           --set         spaces background.color=$COLOR_BACKGROUND
```

</details>

<details><summary>app_space.sh</summary>

```shell
#!/bin/sh

source "$HOME/.config/icons.sh"

# The $SELECTED variable is available for space components and indicates if
# the space invoking this script (with name: $NAME) is currently selected:
# https://felixkratz.github.io/SketchyBar/config/components#space----associate-mission-control-spaces-with-an-item

sketchybar --set $NAME background.drawing=$SELECTED \
	icon.highlight=$SELECTED \
	label.highlight=$SELECTED

if [[ $SENDER == "front_app_switched" || $SENDER == "window_change" ]];
then
 for i in "${!ICONS_SPACE[@]}"
 do
   sid=$(($i+1))
   LABEL=""
 
   QUERY=$(yabai -m query --windows --space $sid)
   APPS=$(echo $QUERY | jq '.[].app')
   TITLES=$(echo $QUERY | jq '.[].title')
 
   if grep -q "\"" <<< $APPS;
   then
     APPS_ARR=()
     while read -r line; do APPS_ARR+=("$line"); done <<< "$APPS"
     TITLES_ARR=()
     while read -r line; do TITLES_ARR+=("$line"); done <<< "$TITLES"
 
     LENGTH=${#APPS_ARR[@]}
     for j in "${!APPS_ARR[@]}"
     do
       APP=$(echo ${APPS_ARR[j]} | sed 's/"//g')
       TITLE=$(echo ${TITLES_ARR[j]} | sed 's/"//g')
 
       ICON=$($HOME/.config/sketchybar/plugins/app_icon.sh "$APP" "$TITLE")
       LABEL+="$ICON"
       if [[ $j < $(($LENGTH-1)) ]]; then
         LABEL+=" "
       fi
     done
   else
     LABEL+="_"
   fi
 
   sketchybar --set space.$sid label="$LABEL"
 done
fi
```

</details>

<details><summary>app_icon.sh</summary>

```shell
#!/bin/sh

source "$HOME/.config/icons.sh"

case "$1" in
"Terminal" | "Warp" | "iTerm2")
  RESULT=$ICON_TERM
	if grep -q "btop" <<< $2;
  then
	 RESULT=$ICON_CHART
	fi
	if grep -q "brew" <<< $2;
  then
	 RESULT=$ICON_PACKAGE
	fi
	if grep -q "nvim" <<< $2;
  then
	 RESULT=$ICON_DEV
	fi
	if grep -q "ranger" <<< $2;
  then
	 RESULT=$ICON_FILE
	fi
	if grep -q "lazygit" <<< $2;
  then
	 RESULT=$ICON_GIT
	fi
	if grep -q "taskwarrior-tui" <<< $2;
  then
	 RESULT=$ICON_LIST
	fi
	if grep -q "unimatrix\|pipes.sh" <<< $2;
  then
	 RESULT=$ICON_SCREENSAVOR
	fi
	if grep -q "bat" <<< $2;
  then
	 RESULT=$ICON_NOTE
	fi
	if grep -q "tty-clock" <<< $2;
  then
	 RESULT=$ICON_CLOCK
	fi
	;;
"Finder")
	RESULT=$ICON_FILE
	;;
"Weather")
	RESULT=$ICON_WEATHER
	;;
"Clock")
	RESULT=$ICON_CLOCK
	;;
"Mail" | "Microsoft Outlook")
	RESULT=$ICON_MAIL
	;;
"Calendar")
	RESULT=$ICON_CALENDAR
	;;
"Calculator" | "Numi")
	RESULT=$ICON_CALC
	;;
"Maps" | "Find My")
	RESULT=$ICON_MAP
	;;
"Voice Memos")
	RESULT=$ICON_MICROPHONE
	;;
"Messages" | "Slack" | "Microsoft Teams" | "Discord" | "Telegram")
	RESULT=$ICON_CHAT
	;;
"FaceTime" | "zoom.us" | "Webex")
	RESULT=$ICON_VIDEOCHAT
	;;
"Notes" | "TextEdit" | "Stickies" | "Microsoft Word")
	RESULT=$ICON_NOTE
	;;
"Reminders" | "Microsoft OneNote")
	RESULT=$ICON_LIST
	;;
"Photo Booth")
	RESULT=$ICON_CAMERA
	;;
"Safari" | "Beam" | "DuckDuckGo" | "Arc" | "Microsoft Edge" | "Google Chrome" | "Firefox")
	RESULT=$ICON_WEB
	;;
"System Settings" | "System Information" | "TinkerTool")
	RESULT=$ICON_COG
	;;
"HOME")
	RESULT=$ICON_HOMEAUTOMATION
	;;
"Music" | "Spotify")
	RESULT=$ICON_MUSIC
	;;
"Podcasts")
	RESULT=$ICON_PODCAST
	;;
"TV" | "QuickTime Player" | "VLC")
	RESULT=$ICON_PLAY
	;;
"Books")
	RESULT=$ICON_BOOK
	;;
"Xcode" | "Code" | "Neovide")
	RESULT=$ICON_DEV
	;;
"Font Book" | "Dictionary")
	RESULT=$ICON_BOOKINFO
	;;
"Activity Monitor")
	RESULT=$ICON_CHART
	;;
"Disk Utility")
	RESULT=$ICON_DISK
	;;
"Screenshot" | "Preview")
	RESULT=$ICON_PREVIEW
	;;
"1Password")
	RESULT=$ICON_PASSKEY
	;;
"NordVPN")
	RESULT=$ICON_VPN
	;;
"Progressive Downloaded" | "Transmission")
	RESULT=$ICON_DOWNLOAD
	;;
"Airflow")
	RESULT=$ICON_CAST
	;;
"Microsoft Excel")
	RESULT=$ICON_TABLE
	;;
"Microsoft PowerPoint")
	RESULT=$ICON_PRESENT
	;;
"OneDrive")
	RESULT=$ICON_CLOUD
	;;
"Curve")
	RESULT=$ICON_PEN
	;;
"VMware Fusion" | "UTM")
	RESULT=$ICON_REMOTEDESKTOP
	;;
*)
	RESULT=$ICON_APP
	;;
esac

echo $RESULT
```

</details>

---

Created by [@hbthen3rd](https://github.com/hbthen3rd)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-6974258)
