# SKHD Modal Indicator

__DESCRIPTION__: Show the current SKHD Mode

__NOTES__: You can hardcode the sketchybar commands into your skhdrc. I just use a seperate helper script to clean things up. 

<details>
   <summary>sketchybarrc</summary>

```SHELL
# M O D A L  I N D I C A T O R 
sketchybar -m --add item modal                           left 
sketchybar -m --set modal icon=[N]
sketchybar -m --set modal icon_color =0xFF83A1F1
```
</details>

<details>
   <summary>skhdrc</summary>

```SHELL
# M O D E S
:: default : $HOME/.config/bin/helpers -n
:: window @ : $HOME/.config/bin/helpers -w
:: scripts @ : $HOME/.config/bin/helpers -s
# Mode Shortcuts
default, scripts < lcmd - escape ; window
window, scripts < escape ; default
default, window < lctrl - escape ; scripts
```
</details>

<details>
   <summary>helpers</summary>

```SHELL
function normal_mode() {
  echo "N O R M A L  M O D E";

  # C O L O R S
  . $HOME/.cache/wal/colors.sh;
  
  # Y A B A I
  yabai -m config active_window_border_color 0xff${color5:1};
  yabai -m config normal_window_border_color 0xff${color8:1};
  yabai -m config insert_feedback_color 0xff${color5:1};

  # S K E T C H Y B A R 
  sketchybar -m --bar color=0xF0${color0:1}
  sketchybar -m --default label.color=0xFF${foreground:1}  
  sketchybar -m --default icon.color=0xFF${color5:1}
  sketchybar -m --set modal icon.color=0xFF83A1F1
  sketchybar -m --set modal icon="[N]"
}

# W I N D O W  M O D E
function window_mode() {
  echo "W I N D O W  M O D E";    

  # C O L O R S
  . $HOME/.cache/wal/colors.sh;

  # Y A B A I
  yabai -m config active_window_border_color 0xff${color3:1};
  yabai -m config normal_window_border_color 0xff${color3:1};
  yabai -m config insert_feedback_color 0xff${color3:1};

  # S K E T C H Y B A R 
  sketchybar -m --bar color=0xF0${color3:1}
  sketchybar -m --default label.color=0xFF${foreground:1}  
  sketchybar -m --default icon.color=0xFF${background:1}
  sketchybar -m --set modal icon.color=0xFFA8CD76
  sketchybar -m --set modal icon="[W]"
}

# S C R I P T S  M O D E
function scripts_mode() {
  echo "S C R I P T S  M O D E";

  # C O L O R S
  . $HOME/.cache/wal/colors.sh;
  
  # Y A B A I
  yabai -m config active_window_border_color 0xff${color5:1};
  yabai -m config normal_window_border_color 0xff${color5:1};
  yabai -m config insert_feedback_color 0xff${color5:1};

  # S K E T C H Y B A R 
  sketchybar -m --bar color=0xF0${color5:1}
  sketchybar -m --default label.color=0xFF${foreground:1}  
  sketchybar -m --default icon.color=0xFF${color6:1}
  sketchybar -m --set modal icon.color=0xFFF29B9B
  sketchybar -m --set modal icon="[S]"
}

```
</details>
Felix: Update to v2.0.0

---

Created by [@typkrft](https://github.com/typkrft)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1267957)
