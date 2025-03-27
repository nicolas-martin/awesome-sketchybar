# Keyboard Layout Indicator
While you could use `alias` to mirror the menu bar keyboard layout icon, it's not customizable with fonts and colors from `sketchybar`. This plugin is a simple implementation that displays the keyboard layout in the menu bar.

<img width="269" alt="image" src="https://github.com/FelixKratz/SketchyBar/assets/22670587/b19b88cb-dd82-48ea-a4b6-4d49d85863b8"><br>
<img width="268" alt="SCR-20231017-otrs" src="https://github.com/FelixKratz/SketchyBar/assets/22670587/d5dd72cc-56eb-4d42-8681-ddebfc93c4fc"><br>
<img width="258" alt="SCR-20231017-otmi" src="https://github.com/FelixKratz/SketchyBar/assets/22670587/15cd9d72-3483-4250-a8ed-d0d81655fd16"><br>

## Caveats
+ I haven't found a way to change the layout like with the vanilla implementation yet.
+ Non-Latin input names do not register properly, and may require more fiddling to get correct.

<details>
  <summary><code>items/keyboard.sh</code></summary>

  ```sh
  #!/bin/bash

  keyboard=(
      icon.drawing=off
      label.font="$FONT:Semibold:14.0"
      script="$PLUGIN_DIR/keyboard.sh"
  )

  sketchybar --add item keyboard right        \
             --set keyboard "${keyboard[@]}"  \
             --add event keyboard_change "AppleSelectedInputSourcesChangedNotification" \
             --subscribe keyboard keyboard_change
  ```
</details>

<details>
  <summary><code>plugins/keyboard.sh</code></summary>

  ```sh
  #!/bin/bash

  # this is jank and ugly, I know
  LAYOUT="$(defaults read ~/Library/Preferences/com.apple.HIToolbox.plist AppleSelectedInputSources | grep "KeyboardLayout Name" | cut -c 33- | rev | cut -c 2- | rev)"

  # specify short layouts individually.
  case "$LAYOUT" in
      "Dvorak") SHORT_LAYOUT="DV";;
      "\"U.S.\"") SHORT_LAYOUT="US";;
      *) SHORT_LAYOUT="한";;
  esac

  sketchybar --set keyboard label="$SHORT_LAYOUT"
  ```
</details>

---

Created by [@bustinbung](https://github.com/bustinbung)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-7308140)
