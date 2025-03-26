# Upcoming Calendar Items Pop Up

https://github.com/user-attachments/assets/f71620c6-9d54-4e9c-ad32-bc5f96677c7f

It currently displays today's and tomorrows events, it can be edited to add a larger range. 

I am sure this script can be improved so if anyone have any ideas please feel free to share. Cheers!

<details>
  <summary>calendar.sh</summary>

``` bash
#!/bin/bash

# === Configuration ===
ICS_URL= "https://your-calendar-url.com/calendar.ics"
ICS_FILE="calendar.ics"

source "$HOME/.config/sketchybar/colors.sh"

# === Download the ICS File ===
curl -s "$ICS_URL" -o "$ICS_FILE"

if [ $? -ne 0 ]; then
  echo "Failed to download the ICS file."
  exit 1
fi

# === Get Today's and Tomorrow's Dates in YYYYMMDD Format ===
TODAY=$(date +"%Y%m%d")
TOMORROW=$(date -v+1d +"%Y%m%d")

declare -a TODAY_EVENTS=()

declare -a TOMORROW_EVENTS=()
echo "Parsing events for today and tomorrow..."

EVENT_DATE=""
EVENT_SUMMARY=""

while IFS= read -r line; do
  # Check for the start of an event
  if [[ "$line" == "BEGIN:VEVENT" ]]; then
    EVENT_DATE=""
    EVENT_SUMMARY=""
  elif [[ "$line" == DTSTART* ]]; then
    # Extract date in YYYYMMDD format
    if [[ "$line" =~ DTSTART[^:]*:([0-9]{8}) ]]; then
      EVENT_DATE="${BASH_REMATCH[1]}"
    elif [[ "$line" =~ DTSTART[^:]*:([0-9]{8})T ]]; then
      EVENT_DATE="${BASH_REMATCH[1]}"
    fi
  elif [[ "$line" == SUMMARY* ]]; then
    # Extract event summary
    EVENT_SUMMARY="${line#SUMMARY:}"
  elif [[ "$line" == "END:VEVENT" ]]; then
    # Check if the event is today or tomorrow
    if [[ "$EVENT_DATE" == "$TODAY" ]]; then
      TODAY_EVENTS+=("$EVENT_SUMMARY")
    fi
    if [[ "$EVENT_DATE" == "$TOMORROW" ]]; then
      # Combine summary and date
      TOMORROW_EVENTS+=("$EVENT_SUMMARY")
    fi
  fi
done <"$ICS_FILE"

# sketchybar --set $NAME icon="􀉉 $(date '+%a %d. %b')" label="$(date '+%I:%M %p')"
sketchybar --set $NAME \
  icon="􀉉 $(date '+%a %d. %b')" \
  label="$(date '+%I:%M %p')" \
  click_script="sketchybar --set $NAME popup.drawing=toggle"

if ((${#TODAY_EVENTS[@]})); then
  sketchybar --add item calendar.date popup.$NAME \
    --set calendar.date \
    label.color=$WHITE \
    label.font.size=14 \
    label.align=left \
    label="Today"

  sketchybar --remove '/^calendar.event[0-9]+$/'
  index=0
  for today_event in "${TODAY_EVENTS[@]}"; do
    index=$((index + 1))
    echo $today_event
    sketchybar --add item calendar.event${index} popup.$NAME \
      --set calendar.event${index} \
      label.color=$WHITE \
      label.font="JetBrainsMono Nerd Font:Italic:12.0" \
      label.max_chars=16 \
      scroll_texts=on \
      label.align=center \
      label="$today_event"
  done
fi

if ((${#TOMORROW_EVENTS[@]})); then
  sketchybar --add item calendar.date2 popup.$NAME \
    --set calendar.date2 \
    label.color=$WHITE \
    label.font.size=14 \
    label.align=left \
    label="$(date -v+1d '+%d. %b ')"

  sketchybar --remove '/^calendar.$NAME[0-9]+$/'
  index=0
  for today_event in "${TOMORROW_EVENTS[@]}"; do
    index=$((index + 1))
    echo $today_event
    sketchybar --add item calendar.tevent${index} popup.$NAME --set calendar.tevent${index} \
      label.color=$WHITE \
      label.font="JetBrainsMono Nerd Font:Italic:12.0" \
      label.max_chars=16 \
      scroll_texts=on \
      label.align=center \
      label="$today_event"
  done
fi
```
</details> 

<details>
  <summary>calendar_item.sh</summary>

``` bash
#!/bin/bash
sketchybar --add item calendar right \
  --set calendar icon=􀉉 \
  update_freq=1 \
  script=$PLUGIN_DIR/calendar.sh \
  label="$(/bin/bash -c "date '+%a %d. %b %I:%M %p'")" \
  background.height=25 \
  click_script="sketchybar --set calendar popup.drawing=toggle" \
  popup.drawing=off \
  padding_left=0 \
  padding_right=0 \
  background.color=0xff232136

sketchybar --set calendar popup.background.border_width=2 \
  popup.background.corner_radius=5 \
  popup.background.border_color=$WHITE \
  popup.background.color=$DARK_BG \
  popup.height=0
```
</details> 



---

Created by [@samodon](https://github.com/samodon)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-10758867)
