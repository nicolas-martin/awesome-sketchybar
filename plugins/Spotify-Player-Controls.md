# Spotify Player Controls
Just a very simple implementation of spotify controls.

This hooks onto the song changed notification spotify send and is thus very lightweight and responsive.
<img width="238" alt="Screen Shot 2021-12-24 at 23 48 42" src="https://user-images.githubusercontent.com/22680421/147373772-82c477e8-81c5-4804-a10d-d58fbe082c61.png">

<details>
  <summary>sketchybarrc</summary>

Setup icons to your liking
```bash
SPOTIFY_EVENT="com.spotify.client.PlaybackStateChanged"
POPUP_SCRIPT="sketchybar -m --set \$NAME popup.drawing=toggle"

sketchybar --add       event           spotify_change $SPOTIFY_EVENT      \
           --add       item            spotify.name center                \
           --set       spotify.name    click_script="$POPUP_SCRIPT"       \
                                       popup.horizontal=on                \
                                       popup.align=center                 \
                                       icon.drawing=off                   \
                                                                          \
           --add       item            spotify.back popup.spotify.name    \
           --set       spotify.back    icon=􀊎                             \
                                       icon.padding_left=5                \
                                       icon.padding_right=5               \
                                       script="$PLUGIN_DIR/spotify.sh"    \
                                       label.drawing=off                  \
           --subscribe spotify.back    mouse.clicked                      \
                                                                          \
           --add       item            spotify.play popup.spotify.name    \
           --set       spotify.play    icon=􀊔                             \
                                       icon.padding_left=5                \
                                       icon.padding_right=5               \
                                       updates=on                         \
                                       label.drawing=off                  \
                                       script="$PLUGIN_DIR/spotify.sh"    \
           --subscribe spotify.play    mouse.clicked spotify_change       \
                                                                          \
           --add       item            spotify.next popup.spotify.name    \
           --set       spotify.next    icon=􀊐                             \
                                       icon.padding_left=5                \
                                       icon.padding_right=10              \
                                       label.drawing=off                  \
                                       script="$PLUGIN_DIR/spotify.sh"    \
           --subscribe spotify.next    mouse.clicked                      \
                                                                          \
           --add       item            spotify.shuffle popup.spotify.name \
           --set       spotify.shuffle icon=􀊝                             \
                                       icon.highlight_color=0xff1DB954    \
                                       icon.padding_left=5                \
                                       icon.padding_right=5               \
                                       label.drawing=off                  \
                                       script="$PLUGIN_DIR/spotify.sh"    \
           --subscribe spotify.shuffle mouse.clicked                      \
                                                                          \
           --add       item            spotify.repeat popup.spotify.name  \
           --set       spotify.repeat  icon=􀊞                             \
                                       icon.highlight_color=0xff1DB954    \
                                       icon.padding_left=5                \
                                       icon.padding_right=5               \
                                       label.drawing=off                  \
                                       script="$PLUGIN_DIR/spotify.sh"    \
           --subscribe spotify.repeat  mouse.clicked
```
</details>
<details>
  <summary>spotify.sh</summary>

Setup icons to your liking
```bash
#!/usr/bin/env sh

next ()
{
  osascript -e 'tell application "Spotify" to play next track'
}

back () 
{
  osascript -e 'tell application "Spotify" to play previous track'
}

play () 
{
  osascript -e 'tell application "Spotify" to playpause'
}

repeat () 
{
  REPEAT=$(osascript -e 'tell application "Spotify" to get repeating')
  if [ "$REPEAT" = "false" ]; then
    sketchybar -m --set spotify.repeat icon.highlight=on
    osascript -e 'tell application "Spotify" to set repeating to true'
  else 
    sketchybar -m --set spotify.repeat icon.highlight=off
    osascript -e 'tell application "Spotify" to set repeating to false'
  fi
}

shuffle () 
{
  SHUFFLE=$(osascript -e 'tell application "Spotify" to get shuffling')
  if [ "$SHUFFLE" = "false" ]; then
    sketchybar -m --set spotify.shuffle icon.highlight=on
    osascript -e 'tell application "Spotify" to set shuffling to true'
  else 
    sketchybar -m --set spotify.shuffle icon.highlight=off
    osascript -e 'tell application "Spotify" to set shuffling to false'
  fi
}

update ()
{
  PLAYING=1
  if [ "$(echo "$INFO" | jq -r '.["Player State"]')" = "Playing" ]; then
    PLAYING=0
    TRACK="$(echo "$INFO" | jq -r .Name | cut -c1-20)"
    ARTIST="$(echo "$INFO" | jq -r .Artist | cut -c1-20)"
    ALBUM="$(echo "$INFO" | jq -r .Album | cut -c1-20)"
    SHUFFLE=$(osascript -e 'tell application "Spotify" to get shuffling')
    REPEAT=$(osascript -e 'tell application "Spotify" to get repeating')
  fi

  args=()
  if [ $PLAYING -eq 0 ]; then
    if [ "$ARTIST" == "" ]; then
      args+=(--set spotify.name label="$TRACK  􀉮  $ALBUM" drawing=on)
    else
      args+=(--set spotify.name label="$TRACK  􀉮  $ARTIST" drawing=on)
    fi
    args+=(--set spotify.play icon=􀊆 \
           --set spotify.shuffle icon.highlight=$SHUFFLE \
           --set spotify.repeat icon.highlight=$REPEAT)
  else
    args+=(--set spotify.name drawing=off \
           --set spotify.name popup.drawing=off \
           --set spotify.play icon=􀊄)
  fi
  sketchybar -m "${args[@]}"
}

mouse_clicked () {
  case "$NAME" in
    "spotify.next") next
    ;;
    "spotify.back") back
    ;;
    "spotify.play") play
    ;;
    "spotify.shuffle") shuffle
    ;;
    "spotify.repeat") repeat
    ;;
    *) exit
    ;;
  esac
}

case "$SENDER" in
  "mouse.clicked") mouse_clicked
  ;;
  "forced") exit
  ;;
  *) update
  ;;
esac
```

</details>

---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1455842)
