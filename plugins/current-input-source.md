# current input source


<details>
<summary>sketchybarrc</summary>

```shell
sketchybar --add event input_change 'AppleSelectedInputSourcesChangedNotification' \
    --add item input \
    --set input script="$plugin_dir/input.sh" \
    --subscribe input input_change
```
</details>


<details>
<summary>input.sh</summary>

for catalina

```shell
#!/usr/bin/env bash

SOURCE=$(defaults read ~/Library/Preferences/com.apple.HIToolbox.plist AppleCurrentKeyboardLayoutInputSourceID)

case ${SOURCE} in
'com.apple.keylayout.ABC') LABEL='A' ;;
'com.apple.keylayout.WubixingKeyboard') LABEL='五' ;;
'com.apple.keylayout.PinyinKeyboard') LABEL='拼' ;;
esac

sketchybar --set $NAME label="$LABEL"

```

for monterey
```shell
#!/usr/bin/env bash

SOURCE=$(defaults read ~/Library/Preferences/com.apple.HIToolbox.plist AppleSelectedInputSources | grep -m1 "Input Mode" | cut -d'"' -f4)

LABEL='A'
case ${SOURCE} in
'com.apple.inputmethod.SCIM.WBX') LABEL='五' ;;
'com.apple.inputmethod.SCIM.ITABC') LABEL='拼' ;;
esac

sketchybar --set $NAME label="$LABEL"

```
</details>


---

Created by [@liangch](https://github.com/liangch)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-2202538)
