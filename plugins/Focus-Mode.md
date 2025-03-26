# Focus Mode
This plugin shows the status of focus mode, and you can click the icon to toggle. This uses macos shortcuts so you'll need to setup a shortcut to toggle focus to make it work properly. Kinda helpful if you're like me and hide the menu bar but want to quickly see the status.

<details>
<summary>sketchybarrc</summary>

```
sketchybar	   --add item focus right					\
		   --set focus							\
			 icon= 						\
			 script="$PLUGIN_DIR/focus.sh"				\
                         click_script="shortcuts run \"toggle focus\""		\
		   --add event focus_on "_NSDoNotDisturbEnabledNotification"	\
		   --add event focus_off "_NSDoNotDisturbDisabledNotification"	\
		   --subscribe focus focus_on focus_off				\
```
</details>

<details>
<summary>focus.sh</summary>

```
#!/bin/sh

status=$(cat ~/Library/DoNotDisturb/DB/Assertions.json | jq .data[0].storeAssertionRecords)

if [ "$status" = "null" ]; then
    sketchybar -m --set focus icon.color=0xFF999999
else
    sketchybar -m --set focus icon.color=0xFFFFFFFF
fi
```
</details>

---

Created by [@mikelu92](https://github.com/mikelu92)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-7720698)
