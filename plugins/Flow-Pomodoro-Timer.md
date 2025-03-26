# Flow Pomodoro Timer

Adds [Flow Pomodoro Timer](https://flowapp.info/) to the bar with some basic functionality. Just make sure you have Flow running when trying to use the plugin. My first time using bash! You can do whatever you want with the plugin.

https://github.com/FelixKratz/SketchyBar/assets/99555305/9882ea1c-b094-4481-8a55-355bf488c148

**sketchybarrc**
```
##### Flow Plugin #####

sketchybar --add item flow center \
           --set flow update_freq=1 \
        	      script="$PLUGIN_DIR/flow.sh" \
	              click_script="sketchybar -m --set flow popup.drawing=toggle" \
	              popup.background.border_width=3 \
                      popup.background.corner_radius=4 \
                      popup.background.border_color=0xFFCA9EE6 \
		      popup.background.color=0xFF737994 \
		      popup.horizontal=on \
		      popup.align=center \
		      \
	   --add item flow.start popup.flow \
	   --set flow.start label="Start" \
	                    click_script="osascript -e 'tell application \"Flow\" to start' ; sketchybar -m --set flow popup.drawing=toggle" \
	   		    \
	   --add item flow.stop popup.flow \
	   --set flow.stop label="Stop" \
	                   click_script="osascript -e 'tell application \"Flow\" to stop' ; sketchybar -m --set flow popup.drawing=toggle" \
	   		   \
	   --add item flow.skip popup.flow \
	   --set flow.skip label="Skip" \
	                   click_script="osascript -e 'tell application \"Flow\" to skip' ; sketchybar -m --set flow popup.drawing=toggle" \
			   \
	   --add item flow.reset popup.flow \
	   --set flow.reset label="Reset" \
	                   click_script="osascript -e 'tell application \"Flow\" to reset' ; sketchybar -m --set flow popup.drawing=toggle" \
			   \
	   --add item flow.show popup.flow \
	   --set flow.show label="Show" \
	                   click_script="osascript -e 'tell application \"Flow\" to show' ; sketchybar -m --set flow popup.drawing=toggle"
```

**flow.sh**
```
#!/bin/bash

TIME=$(osascript -e 'tell application "Flow" to getTime')
PHASE=$(osascript -e 'tell application "Flow" to getPhase') 

if [ $PHASE = "Flow" ]; then
	COLOR=0xFFA6D189
elif [ $PHASE = "Break" ]; then
	COLOR=0xFFE78284
fi

sketchybar --set $NAME label="$TIME $PHASE" icon="󰄉" icon.color=$COLOR
```

---

Created by [@spcoughlin](https://github.com/spcoughlin)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-8034346)
