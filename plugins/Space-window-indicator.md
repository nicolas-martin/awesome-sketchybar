# Space window indicator
<img width="206" alt="Screen Shot 2021-09-06 at 21 41 31" src="https://user-images.githubusercontent.com/9315/132226232-3e77e65e-a5e9-44e8-bf09-13bfafe57dbd.png">

<details>
   <summary>sketchybarrc</summary>

```bash
#!/bin/bash

spaces=()
for i in {1..8}
do
    spaces+=(--add space space$i left \
      --set space$i associated_display=1 associated_space=$i icon=$i)
done

sketchybar -m \
  --add item dummy left \
  --set dummy script="~/.config/sketchybar/plugins/space.sh" \
  --subscribe dummy space_change \
  "${spaces[@]}"
```
</details>

<details>
   <summary>space.sh</summary>

```bash
#!/bin/bash

args=()
while read -r index window
do
  if [ "$window" = "null" ]
  then
    args+=(--set "space${index}" "icon=${index}")
  else
    args+=(--set "space${index}" "icon=${index}°")
  fi
done <<< "$(yabai -m query --spaces | jq -r '.[] | [.index, .windows[0]] | @sh')"

sketchybar -m "${args[@]}"
```


</details>
Felix: Update to v2.0.0

---

Created by [@azuwis](https://github.com/azuwis)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1286504)
