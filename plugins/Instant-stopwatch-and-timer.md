# Instant stopwatch and timer

This is a simple stopwatch and timer plugin for sketchybar.
You can edit presets.
I am willing to accept any modifications!


https://github.com/FelixKratz/SketchyBar/assets/76859067/b653fb80-4363-4dab-8464-06b5a6c1cb56



<details>
<summary>${ITEM_DIR}/timer.sh</summary>

```
#!/usr/bin/env bash

sketchybar --add event reset_timer

timer=(
  script="${PLUGIN_DIR}/reset_timer.sh"
  icon=""
  click_script="sketchybar --set timer popup.drawing=toggle ; sketchybar --trigger reset_timer"
)

stopwatch=(
  click_script="sketchybar --set timer popup.drawing=toggle ; python3 ${PLUGIN_DIR}/timer.py"
  label="SW Mode"
)

preset1=(
  click_script="sketchybar --set timer popup.drawing=toggle ; python3 ${PLUGIN_DIR}/timer.py 180"
  label="3 min"
)

preset2=(
  click_script="sketchybar --set timer popup.drawing=toggle ; python3 ${PLUGIN_DIR}/timer.py 300"
  label="5 min"
)

preset3=(
  click_script="sketchybar --set timer popup.drawing=toggle ; python3 ${PLUGIN_DIR}/timer.py 600"
  label="10 min"
)

preset4=(
  click_script="sketchybar --set timer popup.drawing=toggle ; python3 ${PLUGIN_DIR}/timer.py 1200"
  label="20 min"
)

preset5=(
  click_script="sketchybar --set timer popup.drawing=toggle ; python3 ${PLUGIN_DIR}/timer.py 3600"
  label="1 hour"
)


sketchybar --add item timer left \
             --set timer "${timer[@]}" \
           --subscribe timer reset_timer \
           --add item timer.stopwatch popup.timer \
             --set timer.stopwatch "${stopwatch[@]}" \
           --add item timer.preset1 popup.timer \
             --set timer.preset1 "${preset1[@]}" \
           --add item timer.preset2 popup.timer \
             --set timer.preset2 "${preset2[@]}" \
           --add item timer.preset3 popup.timer \
             --set timer.preset3 "${preset3[@]}" \
           --add item timer.preset4 popup.timer \
             --set timer.preset4 "${preset4[@]}" \
           --add item timer.preset5 popup.timer \
             --set timer.preset5 "${preset5[@]}"
```
</details>

<details>
<summary>${PLUGIN_DIR}/reset_timer.sh</summary>

```
#!/usr/bin/env bash

pid=$(ps -A | grep -v 'grep' | grep -v 'sh' | grep ~/.config/sketchybar/plugins/timer.py | cut -d ' ' -f 1)
kill ${pid}

sketchybar --set timer label=""
```

</details>

<details>
<summary>${PLUGIN_DIR}/timer.py</summary>

```
#!/usr/bin/env python3

# import libs:
import sys
import os
import time


# define colours:
WHITE = str('0xffcad3f5')
RED = str('0xffed8796')


# main function:
def main(argv):
    # set start time:
    start_time = int(time.time())

    # stopwatch mode:
    if len(argv) == 1:
        # start stopwatch:
        stopwatch(start_time)

    # timer mode:
    if len(argv) == 2:
        seconds = int(argv[1])

        # set end time:
        end_time = start_time + seconds

        # start countdown:
        count_down(start_time, end_time)

        # finish message and make a sound:
        finish_event()


def stopwatch(start_time):
    while True:
        current_time = int(time.time())
        delta = current_time - start_time

        os.system('sketchybar --set timer label=' + format_seconds(delta) + ' label.color=' + WHITE)

        time.sleep(1)


def count_down(start_time, end_time):
    delta = end_time - start_time

    while delta > 1:
        current_time = int(time.time())
        delta = end_time - current_time

        # highlight the text if remaining time is less than 60sec:
        if delta < 60:
            color = RED
        else:
            color = WHITE

        os.system('sketchybar --set timer label=' + format_seconds(delta) + ' label.color=' + color)

        time.sleep(1)


def format_seconds(seconds):
    output = ""

    # 3600sec -> 01:00:00
    if seconds >= 3600:
        for duration in (3600, 60, 1):
            output += str(seconds // duration).zfill(2) + ':'
            seconds = seconds % duration

    # 3599sec -> 59:59
    else:
        for duration in (60, 1):
            output += str(seconds // duration).zfill(2) + ':'
            seconds = seconds % duration

    return output.rstrip(':')


def finish_event():
    os.system('sketchybar --set timer label="Time Up!" ' + 'label.color=' + WHITE)

    for i in range(10):
        os.system('afplay /System/Library/Sounds/Funk.aiff')

    os.system('sketchybar --set timer label=""')


if __name__ == '__main__':
    main(sys.argv)
```
</details>

---

Created by [@naba-nyan](https://github.com/naba-nyan)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-7987629)
