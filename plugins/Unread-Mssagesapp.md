## Unread Mssages.app ##

#### Description ####

Displays the total number of unread messages from Apple’s default Messages.app

#### Known Issues ####

So it looks like `messages.sh` gives the correct output when run directly from the terminal but the label does not seem to appear when run from sketchybar.

Also, this would seriously benefit from subscribing to a custom event, but I don’t know how to do that. A more indepth wiki would be beneficial here for plugin developers. I would be more than happy to contribute there in any way that I can!

<details><summary>sketchybarrc</summary><p>

```
sketchybar -m --add item messages right \
              --set messages update_freq=3600 \
              --set messages icon= \
              --set messages script="~/.config/sketchybar/plugins/messages.sh"
```

</p></details>

<details><summary>messages.sh</summary><p>

```
TEXT=$(sqlite3 ~/Library/Messages/chat.db "SELECT text FROM message WHERE is_read=0 AND is_from_me=0 AND text!='' AND date_read=0" | wc -l)

if [ $TEXT = 0 ]; then
  sketchybar -m --set messages drawing=off
else
  sketchybar -m --set messages drawing=on
  sketchybar -m --set messages label=$TEXT 
  echo $TEXT
fi```

</p></details>

---

Created by [@SxC97](https://github.com/SxC97)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1555404)
