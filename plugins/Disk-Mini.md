## Disk Mini ##

#### Description ####

Used [FexliKratz’s](https://github.com/FelixKratz) CPU Mini plugin as a jumping off point and made a plugin to display the memory pressure percentage with a label.

<img width="101" alt="Screen Shot 2021-10-28 at T20 35 28" src="https://user-images.githubusercontent.com/53032923/139358816-64119c77-c1b4-459b-89e5-ce73ea693296.png">

Note: I’m working on porting several menu bar widgets from iStats Menu Bar app to Sketchybar plugins, but i’ve run into a wall trying to get a gpu usage percentage and disk read/write speeds terminal command working. If anyone makes progress on this front, please let me know!

<details><summary>sketchybarrc</summary><p>

```
sketchybar -m --add item ram_label right \
              --set ram_label label.font="FuraCode Nerd Font:Regular:10.0" \
                               label=RAM \
                               y_offset=6 \
                               width=0 \
\
              --add item ram_percentage right \
              --set ram_percentage label.font="FuraCode Nerd Font:Regular:10.0" \
                                    y_offset=-4 \
                                    update_freq=1 \
                                    script="~/.config/sketchybar/plugins/ram.sh"
```

</p></details>

<details><summary>ram.sh</summary><p>

```
sketchybar -m --set ram_percentage label=$(memory_pressure | grep "System-wide memory free percentage:" | awk '{print 100-$5"%"}')
```

</p></details>

---

Created by [@SxC97](https://github.com/SxC97)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1556226)
