# Toggle visibility of a bracket
Naming scheme: the bracket should be named: \<bracket name\> and the item calling the script: \<bracket name\>.\<item name\>

# Example
```bash
sketchybar -m --add       bracket            network                                       \
                                             network.label                                 \
                                             network.vpn                                   \
                                             network.up                                    \
                                             network.down                                  \
                                                                                           \
```
Now if I set `toggle_bracket.sh` as a `click_script` for `network.label` the bracket visibility is toggled on click, but the `network.label` will stay visible. 

<details>
  <summary>toggle_bracket.sh</summary>

```bash
#!/usr/bin/env bash

BRACKET_NAME="$(echo $NAME | tr '.' ' ' | awk "{ print \$1 }")"
ITEMS="$(sketchybar -m --query $BRACKET_NAME | jq -r ".bracket[]")"
args=()
while read -r item
do
  if [ "$item" != "$NAME" ]; then
    args+=(--set "$item" drawing=toggle)
  fi
done <<< "$ITEMS"

sketchybar -m "${args[@]}"
```
</details>

---

Created by [@FelixKratz](https://github.com/FelixKratz)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1774653)
