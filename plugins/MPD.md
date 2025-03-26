# MPD

DESCRIPTION: Display current status of `mpd`
<img width="249" alt="CleanShot 2021-08-28 at 17 13 10@2x" src="https://user-images.githubusercontent.com/184915/131231168-a5e1e760-f312-45d4-9815-1b893a64a9bd.png">

<details>
   <summary>sketchybarrc</summary>

```sh
sketchybar -m --add item mpd right \
              --set mpd update_freq=2 \
              --set mpd script="~/.config/sketchybar/plugins/mpd.sh" \
              --set mpd click_script="mpc toggle"
```
</details>

<details>
   <summary>mpd.sh</summary>

```bash
# #!/usr/bin/env bash

if [ $(mpc status | wc -l | tr -d ' ') == "1" ]; then
  output=""
  icon=""
else
  artist=$(mpc current -f %artist%)
  song=$(mpc current -f %title%)
  status=$(mpc current %state%)

  if [ $status = "playing" ]; then
    icon=""
  else
    icon=""
  fi

  output="${artist} • ${song}"
fi

echo $output
sketchybar -m --set mpd icon="${icon}" \
              --set mpd label="${output}"
```
</details>
Felix: Update to v2.0.0
rhyses-pieces: `mpd.sh` readability improvements

---

Created by [@johnallen3d](https://github.com/johnallen3d)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1248462)
