# Dynamic Popups

I wanted to build popups dynamically, for plugins where I don't know how many items will be in the popup until it's rendered.  E.g., a list of connected (or disconnected) Bluetooth devices, disk usage of currently attached volumes, or calendar events in the next 3 days.

There were a couple technical hurdles to make it work reliably.

1. When removing items from the popup, you need to **--set drawing=off** before **--remove**ing it.
2. Sometimes the item receives **mouse.exited** twice when the cursor exits like in the screenshot.  Not a problem if you don't mind _Set: Item not found_ and _Remove: Item not found_ errors in your log.  Otherwise, try subscribing to **mouse.exited.global** instead, or just use **mouse.clicked**.

    <img width="130" alt="mouse exited" src="https://github.com/user-attachments/assets/eb0db24f-bc41-4242-9623-dbb0c10be43c" />

<details><summary>sketchybarrc</summary>
<p>

```shell
#!/bin/bash

NAME="${NAME:-dynamic_popup}"
POS="${POS:-center}"
CONFIG_DIR="${CONFIG_DIR:-${HOME}/.config/sketchybar}"

sketchybar \
    --add item "${NAME}" "${POS}" \
    --set "${NAME}" label="${NAME}" label.color="0xff000000" background.color="0xffffffff" \
    --set "${NAME}" script="${CONFIG_DIR}/plugins/${NAME}" \
    --subscribe "${NAME}" mouse.clicked mouse.entered mouse.exited.global

```

</p>
</details>

<details><summary>dynamic_popup</summary>
<p>

```shell
#!/bin/bash

make_popup() {
    for i in $(seq 1 5); do
	sketchybar --add item "${NAME}_popup_${i}" "popup.${NAME}" \
		   --set "${NAME}_popup_${i}" label="Item ${i}"
    done

    sketchybar --set "${NAME}" popup.drawing=on
}

clear_popup() {
    sketchybar --query "${NAME}" \
	| jq -r '.popup.items[]' \
	| while read -r; do
	sketchybar --set "${REPLY}" drawing=off --remove "${REPLY}"
    done

    sketchybar --set "${NAME}" popup.drawing=off
}

case "${SENDER}" in
    mouse.clicked)
	POPUP=$(sketchybar --query "${NAME}" | jq -r '.popup.drawing')

	if [ "${POPUP}" == "on" ]; then
	    clear_popup
	else
	    make_popup
	fi
	;;

    mouse.entered)
	POPUP=$(sketchybar --query "${NAME}" | jq -r '.popup.drawing')

	if [ "${POPUP}" == "null" ]; then
	    make_popup
	fi
	;;

    mouse.exited.global)
	POPUP=$(sketchybar --query "${NAME}" | jq -r '.popup.drawing')

	if [ "${POPUP}" == "on" ]; then
	    clear_popup
	fi
	;;
esac

```

</p>
</details>

---

Created by [@ajrosen](https://github.com/ajrosen)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-12568641)
