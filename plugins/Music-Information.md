# Music Information
Update History:
- Felix: Update to v2.0.0
- Brandon: 2/5/23 Attempt to work around music reopening if quit see `FIXME:` in script

__DESCRIPTION__: This will display information about the current track in the Music.app. It will update based on the NSDistributedNotificationCenter Events sent by the Music app.

<details>
   <summary>sketchybarrc</summary>

```SHELL
# Add event
sketchybar -m --add event song_update com.apple.iTunes.playerInfo

# Add Music Item
sketchybar -m --add item music right                         \
    --set music script="~/.config/sketchybar/scripts/music"  \
    click_script="~/.config/sketchybar/scripts/music_click"  \
    label.padding_right=10                                   \
    drawing=off                                              \
    --subscribe music song_update
```
</details>

<details>
   <summary>music</summary>

```SHELL
#!/usr/bin/env bash

# FIXME: Running an osascript on an application target opens that app
# This sleep is needed to try and ensure that theres enough time to
# quit the app before the next osascript command is called. I assume 
# com.apple.iTunes.playerInfo fires off an event when the player quits
# so it imediately runs before the process is killed
sleep 1

APP_STATE=$(pgrep -x Music)
if [[ ! $APP_STATE ]]; then 
    sketchybar -m --set music drawing=off
    exit 0
fi

PLAYER_STATE=$(osascript -e "tell application \"Music\" to set playerState to (get player state) as text")
if [[ $PLAYER_STATE == "stopped" ]]; then
    sketchybar --set music drawing=off
    exit 0
fi

title=$(osascript -e 'tell application "Music" to get name of current track')
artist=$(osascript -e 'tell application "Music" to get artist of current track')
# ALBUM=$(osascript -e 'tell application "Music" to get album of current track')
loved=$(osascript -l JavaScript -e "Application('Music').currentTrack().loved()")
if [[ $loved ]]; then
    icon="􀊸"
fi

if [[ $PLAYER_STATE == "paused" ]]; then
    icon="􀊘"
fi

if [[ $PLAYER_STATE == "playing" ]]; then
    icon="􀊖"
fi

if [[ ${#title} -gt 25 ]]; then
TITLE=$(printf "$(echo $title | cut -c 1-25)…")
fi

if [[ ${#artist} -gt 25 ]]; then
ARTIST=$(printf "$(echo $artist | cut -c 1-25)…")
fi

# if [[ ${#ALBUM} -gt 25 ]]; then
#   ALBUM=$(printf "$(echo $ALBUM | cut -c 1-12)…")
# fi

sketchybar -m --set music icon="$icon"          \
    --set music label="${title} x ${artist}"    \
    --set music drawing=on

```
</details>

<details>
   <summary>music_click</summary>

```APPLESCRIPT
#!/usr/bin/env osascript

tell application "Music"
    if loved of current track is true then
        set loved of current track to false
        do shell script "sketchybar -m --set music icon="
      else
        set loved of current track to true
        do shell script "sketchybar -m --set music icon=􀊸"
    end if
end tell

delay 1

do shell script "sh $HOME/.config/sketchybar/scripts/music"
```
</details>

---

Created by [@typkrft](https://github.com/typkrft)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1216811)
