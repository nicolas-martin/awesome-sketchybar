# Reminders

__DESCRIPTION__: Show your reminders.

__NOTE__: This is just a very basic script, but it could be extended to show the reminder, or use a different app like taskwarrior or taskell instead.

<details>
   <summary>sketchybarrc</summary>

```SHELL
sketchybar -m --add item reminders                       right \
              --set reminders update_freq=20 \
              --set reminders script="~/.config/sketchybar/plugins/reminders.sh" \
              --set reminders click_script="~/.config/sketchybar/plugins/reminders_click.sh"
```
</details>

<details>
   <summary>reminders.sh</summary>

```SHELL
#!/bin/bash

REMINDERS_COUNT=$(osascript -l JavaScript -e "Application('Reminders').lists.byName('📥 Inbox').reminders.whose({completed: false}).name().length")

if [[ $REMINDERS_COUNT -gt 0 ]]; then
  sketchybar -m --set reminders icon="" \
                --set reminders label="$REMINDERS_COUNT"
else
  sketchybar -m --set reminders icon="" \
                --set reminders label=""
fi
```
</details>

<details>
   <summary>reminders_click.sh</summary>

```SHELL
#!/bin/bash

open -a Reminders
```
</details>
Felix: Update to v2.0.0

---

Created by [@typkrft](https://github.com/typkrft)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1222573)
