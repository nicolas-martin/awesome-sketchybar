### Simple Weather from wttr.in

Just set your location as LOCATION in `temp.sh`. There are lots of ways to specify location in wttr (see [help](https://wttr.in/:help) ).

temp.sh
```
#!/usr/bin/env sh
# The $NAME variable is passed from sketchybar and holds the name of
# the item invoking this script:
# https://felixkratz.github.io/SketchyBar/config/events#events-and-scripting
LOCATION="Sydney,%20Australia" # set to your location
TEMP=`curl -s "https://wttr.in/${LOCATION}?format=3" |gsed 's|  *| |g'`
echo $TEMP

sketchybar --set $NAME label="${TEMP}" \
	   --set $NAME click_script="/usr/bin/open /System/Applications/Weather.app"
```

sketchybarrc
```
sketchybar  --add item temp right                              \
                    --set temp update_freq=900                        \
                         script="$PLUGIN_DIR/temp.sh"         
```

---

Created by [@4fthawaiian](https://github.com/4fthawaiian)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-5096270)
