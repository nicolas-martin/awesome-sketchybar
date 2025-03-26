# timer

<img width="214" alt="Screenshot 2025-03-23 at 16 53 27" src="https://github.com/user-attachments/assets/87367e31-b105-4fd9-8aeb-44d7bd1248c3" />

Nothing too fancy but enjoyed getting this timer to work and thought others might want to try it out. The timer counts down. When it's done it will play a sound and you click on it to remove it. Let me know if you try it out!

**This guide assumes you've already installed SketchyBar with a basic config.**

## required pieces
- `sketchybarrc` settings
- timer.end_time file (holds the countdown)
- timer.sh (plugin file)
- timer_end.sh (plugin file)
- command to start the timer.

Run these commands in your terminal to create the directory/files you'll need:
```bash
mkdir ~/.config/sketchybar_helpers
touch ~/.config/sketchybar_helpers/timer.end_time
touch ~/.config/sketchybar/plugins/timer.sh
touch ~/.config/sketchybar/plugins/timer_end.sh
chmod +x ~/.config/sketchybar/plugins/timer.sh
chmod +x ~/.config/sketchybar/plugins/timer_end.sh
```

### `sketchybarrc` settings:
Put this near the top of your file and it will set the default timer length.
```bash
echo "25:00" > ~/.config/sketchybar_helpers/timer.end_time
```
### timer file
You can call this whatever you like, in my case it's called: `timer.end_time` and is empty. I've placed it in it's own directory outside of the `sketchybar` directory so that it doesn't mess with the `--reload` of sketchybar every time something changes.

**Path:** `~/.config/sketchybar_helpers/timer.end_time`

## `timer.sh` plugin
This goes in the plugins folder: `~/.config/sketchybar/plugins/timer.sh`

The important settings are at the top:
- `HELPERS_DIR` is the directory of the `timer.end_time` file
- `END_TIME_FILE` is the file name
- `SOUND` is the audio file you want to play
- `SOUND_DELAY` resets the `update_freq` later in the script so the audio doesn't play too often.

### code:
<details>
<summary>Details</summary>

```bash
#!/bin/sh

HELPERS_DIR="$HOME/.config/sketchybar_helpers"
END_TIME_FILE="$HELPERS_DIR/timer.end_time"
SOUND="Purr"  # Sound to play
SOUND_DELAY=3 # Seconds to wait between sounds repeat

# Read the current time from the file
CURRENT_TIME=$(cat "$END_TIME_FILE")

# Split the time into minutes and seconds
MINUTES=$(echo "$CURRENT_TIME" | cut -d':' -f1)
SECONDS=$(echo "$CURRENT_TIME" | cut -d':' -f2)

# Remove leading zeros to avoid octal interpretation
MINUTES=$(echo "$MINUTES" | sed 's/^0*//')
SECONDS=$(echo "$SECONDS" | sed 's/^0*//')

# Handle empty strings (if all zeros were removed)
[ -z "$MINUTES" ] && MINUTES=0
[ -z "$SECONDS" ] && SECONDS=0

# Calculate the total seconds
TOTAL_SECONDS=$(( MINUTES * 60 + SECONDS ))

# Decrement the total seconds
TOTAL_SECONDS=$(( TOTAL_SECONDS - 1 ))

# Handle negative times (loop back to 59)
if [ "$TOTAL_SECONDS" -lt 0 ]; then
  # Timer has reached 00:00
  # Play sound, if configured
  sketchybar --set timer update_freq=$SOUND_DELAY
    if [ -n "$SOUND" ]; then
      afplay /System/Library/Sounds/"$SOUND".aiff &
    fi

  exit 0
fi

# Calculate the new minutes and seconds
NEW_MINUTES=$(( TOTAL_SECONDS / 60 ))
NEW_SECONDS=$(( TOTAL_SECONDS % 60 ))

# Format with leading zeros
FORMATTED_MINUTES=$(printf "%02d" "$NEW_MINUTES")
FORMATTED_SECONDS=$(printf "%02d" "$NEW_SECONDS")

# Create the new formatted time
NEW_TIME="$FORMATTED_MINUTES:$FORMATTED_SECONDS"

# Write the new time to the file
echo "$NEW_TIME" > "$END_TIME_FILE"

# Update SketchyBar
sketchybar --set timer label="$NEW_TIME"
```

</details> 

## `timer_end.sh` plugin
Used for the `click_script` to remove the timer and reset the time.
### code:
```bash
#!/bin/sh

sketchybar --remove timer
echo "25:00" > ~/.config/sketchybar_helpers/timer.end_time
```
## command to create timer:
I've set up a Shortcut to trigger the timer using this shell command:
```bash
/opt/homebrew/bin/sketchybar --add item timer left --set timer update_freq=1 icon=􁖫 label="25:00" script="~/.config/sketchybar/plugins/timer.sh" click_script="~/.config/sketchybar/plugins/timer_end.sh"
```



---

Created by [@benjaminwelch](https://github.com/benjaminwelch)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-12595672)
