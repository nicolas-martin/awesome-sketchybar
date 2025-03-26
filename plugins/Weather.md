# Weather

## Deprecated

Please see @es183923 [improved weather widget](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1634025) below. Their version is much more thorough! Bonus points to @leaf-ts for providing the tip to [detect your current location](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-2989126).

## Old Script

DESCRIPTION: Display current weather conditions and temperature.
<img width="81" alt="CleanShot 2021-08-28 at 17 19 19@2x" src="https://user-images.githubusercontent.com/184915/131231271-65a963ad-ed02-4194-b212-51c8f331442e.png">

<details>
   <summary>sketchybarrc</summary>

```sh
sketchybar -m --add item weather right \
              --set weather update_freq=1800 \
              --set weather script="~/.config/sketchybar/plugins/weather.sh" \
              --set weather click_script="open https://darksky.net/forecast/40.7127,-74.0059/us12/en"
```
</details>

<details>
   <summary>weather.sh</summary>

```bash
#!/usr/bin/env bash

status=$(curl -s 'wttr.in/NewYork?format=%C+|+%t')
condition=$(echo $status | awk -F '|' '{print $1}' | tr '[:upper:]' '[:lower:]')
condition="${condition// /}"
temp=$(echo $status | awk -F '|' '{print $2}')
temp="${temp//\+/}"
temp="${temp// /}"

# add more conditions here as appropriate
case "${condition}" in
  "sunny")
    icon=""
    ;;
  "partlycloudy")
    icon=""
    ;;
  *)
    icon="$condition"
    ;;
esac

sketchybar -m --set weather icon="$icon" \
              --set weather label="$temp"

```
</details>
Felix: Update to v2.0.0

---

Created by [@johnallen3d](https://github.com/johnallen3d)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1248469)
