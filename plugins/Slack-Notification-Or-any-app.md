# Slack Notification (Or any app)

<img width="234" alt="Capture d’écran 2023-08-29 à 22 39 46" src="https://github.com/FelixKratz/SketchyBar/assets/21695361/6c6e5ccc-2ff9-4cfd-bb65-272b74d71775">
<img width="234" alt="Capture d’écran 2023-08-29 à 22 40 13" src="https://github.com/FelixKratz/SketchyBar/assets/21695361/a822fcef-f1ff-4068-9784-b2aeccf213c2">
<img width="236" alt="Capture d’écran 2023-08-29 à 22 40 30" src="https://github.com/FelixKratz/SketchyBar/assets/21695361/99ece6a6-c0b1-4ecc-90a6-a90ee0469d87">

This retrieves the StatusLabel of the app, you can change the name of the app in `STATUS_LABEL`. If you have a better way to retrieves Slack notifs let me know! 

<details>
<summary>slack.sh</summary>

```bash
#!/usr/bin/env sh
STATUS_LABEL=$(lsappinfo info -only StatusLabel "Slack")
ICON="󰒱"
if [[ $STATUS_LABEL =~ \"label\"=\"([^\"]*)\" ]]; then
    LABEL="${BASH_REMATCH[1]}"

    if [[ $LABEL == "" ]]; then
        ICON_COLOR="0xffa6da95"
    elif [[ $LABEL == "•" ]]; then
        ICON_COLOR="0xffeed49f"
    elif [[ $LABEL =~ ^[0-9]+$ ]]; then
        ICON_COLOR="0xffed8796"
    else
        exit 0
    fi
else
  exit 0
fi

sketchybar --set $NAME icon=$ICON label="${LABEL}" icon.color=${ICON_COLOR}
```

</details>

<details>
<summary>sketchybar</summary>

```bash
sketchybar  --add   item slack right \
            --set   slack \
                    update_freq=180 \
                    script="$PLUGIN_DIR/slack.sh" \
                    background.padding_left=15  \
                    icon.font.size=18 \
           --subscribe slack system_woke
```

</details>

---

Created by [@Max0u](https://github.com/Max0u)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-6857450)
