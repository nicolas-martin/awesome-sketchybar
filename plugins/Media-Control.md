# Media Control

This plugin is a mix between [`Now Playing Info`](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-5730038) and [`Spotify`](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1455842), it basically shows the current media playing (from any app) and gives you simple controls when hovered.

![animation](https://github.com/FelixKratz/SketchyBar/assets/9378265/90fd63eb-46f2-40e2-a554-fabbd0fe892f)


(The `Arc` on the left is my web browser, it essentially shows the source app of whatever is playing) 

It uses [`nowplaying-cli`](https://github.com/kirtan-shah/nowplaying-cli) to get the now-playing info (note that it needs to be installed to `~/.local/bin/` for the plugin to work out of the box).

<details>
  <summary>sketchybarrc</summary>
  
  ```shell
 #!/bin/bash

POPUP_SCRIPT="sketchybar -m --set media_ctrl.anchor popup.drawing=toggle"

media_ctrl_anchor=(
  script="$PLUGIN_DIR/media_ctrl.sh"
  click_script="$POPUP_SCRIPT"
  popup.horizontal=on
  popup.align=center
  popup.height=150
)

media_ctrl_cover=(
  script="$PLUGIN_DIR/media_ctrl.sh"
  click_script="open -a 'Arc'; $POPUP_SCRIPT"
  label.drawing=off
  icon.drawing=off
  padding_left=12
  padding_right=10
  background.image.scale=0.6
  background.image.drawing=on
  background.drawing=on
)

media_ctrl_title=(
  icon.drawing=off
  padding_left=0
  padding_right=0
  width=0
  label.font="$FONT:Heavy:15.0"
  label.max_chars=25
  y_offset=55
)

media_ctrl_artist=(
  icon.drawing=off
  y_offset=30
  padding_left=0
  padding_right=0
  width=0
  label.max_chars=20
)

media_ctrl_album=(
  icon.drawing=off
  padding_left=0
  padding_right=0
  y_offset=15
  width=0
  label.max_chars=30
)

media_ctrl_back=(
  icon=􀊎
  icon.padding_left=5
  icon.padding_right=5
  icon.color=$BLACK
  script="$PLUGIN_DIR/media_ctrl.sh"
  label.drawing=off
  y_offset=-45
)

media_ctrl_play=(
  icon=􀊔
  background.height=40
  background.corner_radius=20
  width=100
  align=center
  background.color=$POPUP_BACKGROUND_COLOR
  background.border_color=$WHITE
  background.border_width=0
  background.drawing=on
  icon.padding_left=4
  icon.padding_right=5
  updates=on
  label.drawing=off
  script="$PLUGIN_DIR/media_ctrl.sh"
  y_offset=-45
)

media_ctrl_next=(
  icon=􀊐
  icon.padding_left=5
  icon.padding_right=5
  icon.color=$BLACK
  label.drawing=off
  script="$PLUGIN_DIR/media_ctrl.sh"
  y_offset=-45
)

media_ctrl_controls=(
  background.color=$GREEN
  background.corner_radius=11
  background.drawing=on
  y_offset=-45
)

sketchybar --add item media_ctrl.anchor center                      \
           --set media_ctrl.anchor "${media_ctrl_anchor[@]}"           \
           --subscribe media_ctrl.anchor mouse.entered mouse.exited \
                                      mouse.exited.global media_change \
                                                                 \
           --add item media_ctrl.cover popup.media_ctrl.anchor         \
           --set media_ctrl.cover "${media_ctrl_cover[@]}"             \
                                                                 \
           --add item media_ctrl.title popup.media_ctrl.anchor         \
           --set media_ctrl.title "${media_ctrl_title[@]}"             \
                                                                 \
           --add item media_ctrl.artist popup.media_ctrl.anchor        \
           --set media_ctrl.artist "${media_ctrl_artist[@]}"           \
                                                                 \
           --add item media_ctrl.album popup.media_ctrl.anchor         \
           --set media_ctrl.album "${media_ctrl_album[@]}"             \
                                                                 \
           --add item media_ctrl.back popup.media_ctrl.anchor          \
           --set media_ctrl.back "${media_ctrl_back[@]}"               \
           --subscribe media_ctrl.back mouse.clicked                \
                                                                 \
           --add item media_ctrl.play popup.media_ctrl.anchor          \
           --set media_ctrl.play "${media_ctrl_play[@]}"               \
           --subscribe media_ctrl.play mouse.clicked media_change   \
                                                                 \
           --add item media_ctrl.next popup.media_ctrl.anchor          \
           --set media_ctrl.next "${media_ctrl_next[@]}"               \
           --subscribe media_ctrl.next mouse.clicked                \
                                                                 \
           --add item media_ctrl.spacer popup.media_ctrl.anchor        \
           --set media_ctrl.spacer width=5                          \
                                                                 \
           --add bracket media_ctrl.controls media_ctrl.back           \
                                          media_ctrl.play           \
                                          media_ctrl.next           \
           --set media_ctrl.controls "${media_ctrl_controls[@]}"
           
  ```
  
</details>

<details>
<summary>media_ctrl.sh</summary>

```shell
#!/bin/bash

next ()
{
  ~/.local/bin/nowplaying-cli next
}

back () 
{
  ~/.local/bin/nowplaying-cli previous
}

play () 
{
  ~/.local/bin/nowplaying-cli togglePlayPause
}

update ()
{
  PLAYING=1
  TRACK="$(~/.local/bin/nowplaying-cli get title)"
  if [ "$TRACK" != "null" ]; then
    PLAYING=0
    ARTIST="$(~/.local/bin/nowplaying-cli get artist)"
    ALBUM="$(~/.local/bin/nowplaying-cli get album)"
    APP="$(base64 -d <<< $(~/.local/bin/nowplaying-cli get clientPropertiesData) | awk -F ':' '{print $2}')"
    MEDIA="$APP: $TRACK - $ARTIST"
    PLAYBACK_RATE=$(~/.local/bin/nowplaying-cli get playbackRate)
  fi

  args=()
  if [ $PLAYING -eq 0 ]; then
    ~/.local/bin/nowplaying-cli get artworkData | base64 --decode > /tmp/cover.jpg
    if [ "$ARTIST" == "" ]; then
      args+=(--set media_ctrl.title label="$TRACK"
             --set media_ctrl.album label="Podcast"
             --set media_ctrl.artist label="$ALBUM"  )
    else
      args+=(--set media_ctrl.title label="$TRACK"
             --set media_ctrl.album label="$ALBUM"
             --set media_ctrl.artist label="$ARTIST")
    fi
    args+=(--set media_ctrl.play icon=􀊆
           --set media_ctrl.cover background.image="/tmp/cover.jpg"
                               background.color=0x00000000
           --set media_ctrl.anchor label="$MEDIA" drawing=on                      )
    if [ "$PLAYBACK_RATE" -eq 0 ]; then
      args+=(--set media_ctrl.play icon=􀊄                         )
    fi
  fi
  sketchybar -m "${args[@]}"
}

mouse_clicked () {
  case "$NAME" in
    "media_ctrl.next") next
    ;;
    "media_ctrl.back") back
    ;;
    "media_ctrl.play") play
    ;;
    *) exit
    ;;
  esac
}

popup () {
  sketchybar --set media_ctrl.anchor popup.drawing=$1
}

routine() {
  case "$NAME" in
    *) update
    ;;
  esac
}

case "$SENDER" in
  "mouse.clicked") mouse_clicked
  ;;
  "mouse.entered") popup on
  ;;
  "mouse.exited"|"mouse.exited.global") popup off
  ;;
  "routine") routine
  ;;
  "forced") exit 0
  ;;
  *) update
  ;;
esac

```
</details>

I just discovered `Sketchybar` (thanks @FelixKratz it is amazing!), so feel free to suggest improvements as my code is essentially a (dirty) merge between the `media` and `spotify` plugins. 

---

Created by [@charlesbvll](https://github.com/charlesbvll)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-7533397)
