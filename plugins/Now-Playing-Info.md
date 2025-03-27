# Now Playing Info
This demo uses the experimental `media_change` event currently only available on master, but will be included in 2.15.2.

The `$INFO` variable of the `media_change` event holds info about the currently playing media. This works for Spotify, Safari, Music and probably most other media apps as well:

sketchybarrc:
```bash
media=(
  script="$PLUGIN_DIR/media.sh"
  updates=on
)

sketchybar --add item media center \
           --set media "${media[@]}" \
           --subscribe media media_change
```

media.sh:
```bash
#!/bin/bash
STATE="$(echo "$INFO" | jq -r '.state')"

if [ "$STATE" = "playing" ]; then
  MEDIA="$(echo "$INFO" | jq -r '.app + ": " + .title + " - " + .artist')"
  sketchybar --set $NAME label="$MEDIA" drawing=on
else
  sketchybar --set $NAME drawing=off
fi

---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-5730038)
