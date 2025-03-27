# Space Indicator

I know this is part of some other plugins in here but I wanted something super simple to track spaces with Yabai. 
If this is redundant feel free to take it down

![Screen Shot 2022-09-16 at 4 55 20 PM](https://user-images.githubusercontent.com/61203664/190732717-ec9fb94e-bea8-4329-a6c6-912684496693.png)

<details>
<summary>sketchybarrc</summary>

```
sketchybar -m --add item space_info left \
--set space_info script="$PLUGINS/space_info.sh" \
--subscribe space_info space_change
```
</details>

<details>
<summary>space_info.sh</summary>

```bash
#!/bin/bash

ACTIVE_SPACE=$(echo $INFO | jq -r '.["display-1"]')
ALL_SPACES=$(/opt/homebrew/bin/yabai -m query --spaces | jq -r '.[] | .index' | tr '\n' ' ')
ALL_SPACES=($ALL_SPACES)

if [ "${#ALL_SPACES[@]}" -eq 1 ]; then
  exit
fi

if [[ "$ACTIVE_SPACE" == "" ]]; then
  ACTIVE_SPACE=$(/opt/homebrew/bin/yabai -m query --spaces --space | jq '.index')
fi

LABEL=()

for i in "${ALL_SPACES[@]}"
do
  if [ "$i" = "$ACTIVE_SPACE" ]; then
    LABEL+=(">$i<")
  else
    LABEL+=("$i")
  fi
done

LABEL=$(echo "${LABEL[*]}")

sketchybar -m --set space_info label="$LABEL"
```
</details>

---

Created by [@sean-hale-dev](https://github.com/sean-hale-dev)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-3665730)
