# Combined Wifi Status, IP Address, and VPN status

## Three states:

### Not connected:

<img width="273" alt="skb_not-connected" src="https://github.com/FelixKratz/SketchyBar/assets/13052949/05add27f-f0a5-400a-8d05-f3705d3ae318">

### Connected (IP Address):

<img width="269" alt="skb_connected-ip" src="https://github.com/FelixKratz/SketchyBar/assets/13052949/9f747fac-ee31-4ddd-aba5-0ab93a6d9cb0">

### Connected to VPN:

<img width="204" alt="skb_connected-vpn" src="https://github.com/FelixKratz/SketchyBar/assets/13052949/30c98790-dff0-48f8-8050-b5e74b1dde08">

<details><summary>sketchybarrc</summary>

```shell
# Bar

sketchybar --bar color=0x00000000 \
                 height=56        \
                 margin=0         \
                 y_offset=0       \
                 padding_left=8   \
                 padding_right=8

# Defaults

sketchybar --default padding_left=8                                \
                     padding_right=8                               \
                                                                   \
                     background.border_color=$YELLOW               \
                     background.border_width=2                     \
                     background.height=40                          \
                     background.corner_radius=12                   \
                                                                   \
                     icon.color=$YELLOW                            \
                     icon.highlight_color=$BACKGROUND              \
                     icon.padding_left=6                           \
                     icon.padding_right=2                          \
                     icon.font="CaskaydiaCove Nerd Font:Book:16.0" \
                                                                   \
                     label.color=$YELLOW                           \
                     label.highlight_color=$BACKGROUND             \
                     label.padding_left=2                          \
                     label.padding_right=6                         \
                     label.font="SF Pro:Semibold:12.0"

# Items

sketchybar --add item  ip_address right                              \
           --set       ip_address script="$PLUGIN_DIR/ip_address.sh" \
                                  update_freq=30                     \
                                  padding_left=2                     \
                                  padding_right=8                    \
                                  background.border_width=0          \
                                  background.corner_radius=6         \
                                  background.height=24               \
                                  icon.highlight=on                  \
                                  label.highlight=on                 \
           --subscribe ip_address wifi_change                        \
                                                                     \
           --add item  volume right                                  \
           --set       volume script="$PLUGIN_DIR/volume.sh"         \
                              padding_left=2                         \
                              padding_right=2                        \
                              background.border_width=0              \
                              background.height=24                   \
           --subscribe volume volume_change                          \
                                                                     \
           --add item  battery right                                 \
           --set       battery script="$PLUGIN_DIR/battery.sh"       \
                               update_freq=120                       \
                               padding_left=8                        \
                               padding_right=2                       \
                               background.border_width=0             \
                               background.height=24                  \
           --subscribe battery system_woke power_source_change

# Bracket

sketchybar --add bracket status ip_address volume battery     \
           --set         status background.color=$BACKGROUND  \
                                background.border_color=$BLUE
```

</details> 

<details><summary>ip_address.sh</summary>

```shell
#!/bin/sh

source "$HOME/.config/colors.sh" # Loads all defined colors

IP_ADDRESS=$(scutil --nwi | grep address | sed 's/.*://' | tr -d ' ' | head -1)
IS_VPN=$(scutil --nwi | grep -m1 'utun' | awk '{ print $1 }')

if [[ $IS_VPN != "" ]]; then
	COLOR=$CYAN
	ICON=
	LABEL="VPN"
elif [[ $IP_ADDRESS != "" ]]; then
	COLOR=$BLUE
	ICON=
	LABEL=$IP_ADDRESS
else
	COLOR=$WHITE
	ICON=
	LABEL="Not Connected"
fi

sketchybar --set $NAME background.color=$COLOR \
	icon=$ICON \
	label="$LABEL"
```

</details> 

---

# Bonus:

## With Network status:

<img width="316" alt="skb_network" src="https://github.com/FelixKratz/SketchyBar/assets/13052949/6d8be62f-fd40-4dd2-8c57-f4efa1ea61fc">

<details><summary>sketchybarrc</summary>

```shell
# Bar

sketchybar --bar color=0x00000000 \
                 height=56        \
                 margin=0         \
                 y_offset=0       \
                 padding_left=8   \
                 padding_right=8

# Defaults

sketchybar --default padding_left=8                                \
                     padding_right=8                               \
                                                                   \
                     background.border_color=$YELLOW               \
                     background.border_width=2                     \
                     background.height=40                          \
                     background.corner_radius=12                   \
                                                                   \
                     icon.color=$YELLOW                            \
                     icon.highlight_color=$BACKGROUND              \
                     icon.padding_left=6                           \
                     icon.padding_right=2                          \
                     icon.font="CaskaydiaCove Nerd Font:Book:16.0" \
                                                                   \
                     label.color=$YELLOW                           \
                     label.highlight_color=$BACKGROUND             \
                     label.padding_left=2                          \
                     label.padding_right=6                         \
                     label.font="SF Pro:Semibold:12.0"

# Items

sketchybar --add item  ip_address right                              \
           --set       ip_address script="$PLUGIN_DIR/ip_address.sh" \
                                  update_freq=30                     \
                                  padding_left=2                     \
                                  padding_right=8                    \
                                  background.border_width=0          \
                                  background.corner_radius=6         \
                                  background.height=24               \
                                  icon.highlight=on                  \
                                  label.highlight=on                 \
           --subscribe ip_address wifi_change                        \
                                                                     \
           --add item  network.up right                              \
           --set       network.up script="$PLUGIN_DIR/network.sh"    \
                                  update_freq=20                     \
                                  padding_left=2                     \
                                  padding_right=2                    \
                                  background.border_width=0          \
                                  background.height=24               \
                                  icon=⇡                             \
                                  icon.color=$YELLOW                 \
                                  label.color=$YELLOW                \
                                                                     \
           --add item  network.down right                            \
           --set       network.down script="$PLUGIN_DIR/network.sh"  \
                               update_freq=20                        \
                               padding_left=8                        \
                               padding_right=2                       \
                               background.border_width=0             \
                               background.height=24                  \
                               icon=⇣                                \
                               icon.color=$GREEN                     \
                               label.color=$GREEN

# Bracket

sketchybar --add bracket status ip_address network.up network.down   \
           --set         status background.color=$BACKGROUND         \
                                background.border_color=$BLUE
```

</details> 

<details><summary>network.sh</summary>

```shell
#!/bin/sh

function getBytes {
    netstat -w1 > ~/.config/sketchybar/plugins/network.out & sleep 1; kill $!
}

BYTES=$(getBytes > /dev/null)
BYTES=$(cat ~/.config/sketchybar/plugins/network.out | grep '[0-9].*')

DOWN=$(echo $BYTES | awk '{print $3}')
UP=$(echo $BYTES | awk '{print $6}')

function human_readable() {
    local abbrevs=(
        $((1 << 60)):ZiB
        $((1 << 50)):EiB
        $((1 << 40)):TiB
        $((1 << 30)):GiB
        $((1 << 20)):MiB
        $((1 << 10)):KiB
        $((1)):B
    )

    local bytes="${1}"
    local precision="${2}"

    for item in "${abbrevs[@]}"; do
        local factor="${item%:*}"
        local abbrev="${item#*:}"
        if [[ "${bytes}" -ge "${factor}" ]]; then
            local size="$(bc -l <<< "${bytes} / ${factor}")"
            printf "%.*f %s\n" "${precision}" "${size}" "${abbrev}"
            break
        fi
    done
}

DOWN_FORMAT=$(human_readable $DOWN 1)
UP_FORMAT=$(human_readable $UP 1)

sketchybar --set network.down label="$DOWN_FORMAT/s" \
	       --set network.up   label="$UP_FORMAT/s"
```

</details> 

---

Created by [@hbthen3rd](https://github.com/hbthen3rd)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-6793056)
