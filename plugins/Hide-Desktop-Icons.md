
### Hide Desktop Icons
Hide all desktop icons by clicking on the little icon in the menu bar. Useful for talks where you may not want to share your desktop content with everyone in the audience. 

**Keep in mind that clicking on the button kills Finder.**

*desktop visible state*
<img width="21" alt="grafik" src="https://user-images.githubusercontent.com/50205381/215050770-cc232e3b-0a6c-4d76-ad3c-f46ccf318382.png"> 

*desktop hidden state*
<img width="21" alt="grafik" src="https://user-images.githubusercontent.com/50205381/215050873-0772d057-3ca8-4039-a7fa-72d2aff3a077.png">



<details>
<summary>sketchybarrc</summary>
<br>

```
sketchybar --add  item  hide  right                                 \
           --set  hide        click_script="$PLUGIN_DIR/hide.sh"    \
                              icon.drawing=off                      \
                              label.font="$ICON_FONT:Regular:16.0"  \
                              label.drawing=on                      \
                              label="❚"
```

</details>
<details>
<summary>hide.sh</summary>
<br>

```
#!/usr/bin/env bash

DESKTOP=$(defaults read com.apple.finder CreateDesktop)

if [[ $DESKTOP == "true" ]]; then 
    sketchybar -m --set $NAME label="❚" label.color=0x33ffffff
    defaults write com.apple.finder CreateDesktop false
    killall Finder
else 
    sketchybar -m --set $NAME label="❚" label.color=0xffffffff
    defaults write com.apple.finder CreateDesktop true
    killall Finder
fi
```
</details>


---

Created by [@akarsai](https://github.com/akarsai)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-4795057)
