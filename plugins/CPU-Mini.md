# CPU Mini
Small Item with a vertically stacked CPU label above the CPU percentage.
![Screen Shot 2021-09-24 at 00 45 21](https://user-images.githubusercontent.com/22680421/134594612-9abbe9ea-00f0-4f84-9dbd-97ed04618575.jpg)
<details>
  <summary>sketchybarrc</summary>

```bash
sketchybar -m --add       item               cpu_label right                                               \
              --set       cpu_label          label.font="SF Pro:Semibold:7"                                \
                                             label=CPU                                                     \
                                             y_offset=6                                                    \
                                             width=0                                                       \
                                                                                                           \
              --add       item               cpu_percent right                                             \
              --set       cpu_percent        label.font="SF Pro:Heavy:12"                                  \
                                             y_offset=-4                                                   \
                                             update_freq=2                                                 \
                                             script="~/.config/sketchybar/plugins/cpu.sh"
```
</details>

<details>
  <summary>cpu.sh</summary>

```bash
#!/usr/bin/env bash

CORE_COUNT=$(sysctl -n machdep.cpu.thread_count)
CPU_INFO=$(ps -eo pcpu,user)
CPU_SYS=$(echo "$CPU_INFO" | grep -v $(whoami) | sed "s/[^ 0-9\.]//g" | awk "{sum+=\$1} END {print sum/(100.0 * $CORE_COUNT)}")
CPU_USER=$(echo "$CPU_INFO" | grep $(whoami) | sed "s/[^ 0-9\.]//g" | awk "{sum+=\$1} END {print sum/(100.0 * $CORE_COUNT)}")

sketchybar -m --set  cpu_percent label=$(echo "$CPU_SYS $CPU_USER" | awk '{printf "%.0f\n", ($1 + $2)*100}')%
```
</details>

---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1377124)
