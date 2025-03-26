# Show Keyboard Layout

Shows the current keyboard layout. 
![image](https://user-images.githubusercontent.com/50205381/175072734-e4d1b31d-6735-4cf2-8fc1-dcb12cd23ce7.png)
![image](https://user-images.githubusercontent.com/50205381/175072766-881cf95b-7c53-4aae-a19c-9d4d5d701d4d.png)

In this form, only German and English layout is supported. In the MacOS settings, the English keyboard has to be of type "ABC", and the German keyboard has to be registered as German. 
![image](https://user-images.githubusercontent.com/50205381/175072400-78122eb5-9398-4db4-8dbc-84ba28851fdd.png)

Use the terminal command 
`defaults read ~/Library/Preferences/com.apple.HIToolbox.plist AppleCurrentKeyboardLayoutInputSourceID`
to look up the names of the input methods of choice, and adapt 'keyboard.sh' accordingly.

<details><summary>sketchybarrc</summary>
<p>

```bash
!/usr/bin/env sh

sketchybar --add       event              input_change 'AppleSelectedInputSourcesChangedNotification' \
           --add       item               keyboard right                                              \
           --set       keyboard           script="$PLUGIN_DIR/keyboard.sh"                            \
                                          width=44                                                    \
                                          label.font="$FONT:Regular Italic:14.0"                      \
           --subscribe keyboard           input_change                        


```

</p>
</details>

<details><summary>keyboard.sh</summary>
<p>

```bash
#!/usr/bin/env bash

SOURCE=$(defaults read ~/Library/Preferences/com.apple.HIToolbox.plist AppleCurrentKeyboardLayoutInputSourceID)

case ${SOURCE} in
'com.apple.keylayout.ABC') LABEL='en' ;;
'com.apple.keylayout.German') LABEL='de' ;;
esac

sketchybar --set $NAME label="$LABEL"
```

</p>
</details>



---

Created by [@akarsai](https://github.com/akarsai)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-3003484)
