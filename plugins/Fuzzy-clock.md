### Fuzzy clock
Clock that rounds to the nearest 5 minutes.

<img width="84" alt="grafik" src="https://user-images.githubusercontent.com/50205381/199709764-d684d643-07d3-4c4f-ba63-a32e3809d5da.png"> <img alt="grafik" src="https://user-images.githubusercontent.com/50205381/199710519-45ea6e1a-cf8b-48b6-b6bb-d24702fb6921.png">


<details>
<summary>sketchybarrc</summary>
<br>

```
sketchybar --add       item               calendar.time right                           \
           --set       calendar.time      update_freq=10                                \
                                          icon.drawing=off                              \
                                          script="$PLUGIN_DIR/time.sh"                  \
```

</details>
<details>
<summary>time.sh</summary>
<br>

```
#!/usr/bin/env bash

mins=$(date '+%M')
hours=$(date '+%H')
fuzzymins=$(awk '{for (i=1; i<=NF; i++) $i = int( ($i+2) / 5) * 5} 1' <<< "$mins")

# handle special cases
if [ $fuzzymins -gt 55 ]; then # greater than 55
    fuzzymins="00"
    hours=$(date -v+1H '+%H')
elif [ ${#fuzzymins} -eq 1 ]; then # only one digit (0 or 5)
    fuzzymins="0$fuzzymins"
fi

sketchybar --set $NAME label="$hours:$fuzzymins"
```
</details>



---

Created by [@akarsai](https://github.com/akarsai)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-4046227)
