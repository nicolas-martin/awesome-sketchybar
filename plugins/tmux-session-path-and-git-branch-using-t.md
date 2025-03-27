# tmux session path and git branch using tmux hooks

My workflow relies on tmux, and switching between various sessions (one for each project). I use my own little [sessionizer script](https://github.com/oschrenk/dotfiles/blob/master/home/private_dot_config/tmux/executable_fzf_sessions.fish) that offers sessions based on git directories within `$HOME/Projects` and gives these sessions appropriate names and session paths.

I had a tmux status bar that told me the current project, git branch, jira tickets asigned to me, and Github reviews assigned to me.

I now replaced it with sketchybar. 

https://github.com/FelixKratz/SketchyBar/assets/52898/f758a204-f071-40b5-b861-1de6573515dd


The plugin is purely event based thanks to tmux hooks.

Again: This setup relies on tmux sessions having a session path associated with them. I also personally have a "persistent" session called `default` which is not associated with a git project (and thus no session path).

<details>
  <summary>tmux.conf</summary>
  
```
# add this hook to your tmux.conf
# if the session changes we send an event to sketchybar
set-hook -g client-session-changed  'run-shell "sketchybar --trigger tmux_session_update"'
```

</details>

<details>
  <summary> sketchybar </summary>
  
```
sketchybar --add item tmux.attached left \
		       --set tmux.attached \
			     icon.drawing=off  \
			     script="$PLUGIN_DIR/tmux.attached.sh"				\
		       --add event tmux_session_update \
		       --subscribe tmux.attached tmux_session_update

sketchybar --add item tmux.git left \
		       --set tmux.git \
			     icon.drawing=off  \
			     script="$PLUGIN_DIR/tmux.git.sh"				\
		       --add event tmux_session_update \
		       --subscribe tmux.git tmux_session_update
```

</details>

<details>
  <summary>$PLUGIN_DIR/tmux.attached.sh</summary>
  
```
#!/bin/sh

session_raw=$(tmux list-sessions -F '#{session_name}:#{session_path}' -f "#{==:#{session_attached},1}")
session_name=$(echo "$session_raw" | cut -d ":" -f 1)
session_path=$(echo "$session_raw" | cut -d ":" -f 2)

if [ "$SENDER" = "tmux_session_update" ]; then
  if [ "$session_name" = "default" ]; then
    sketchybar --set "$NAME" drawing=off 
  else 
    sketchybar --set "$NAME" drawing=on icon.drawing=on icon= label="$session_name"
  fi
fi
```

</details>

<details>
  <summary>$PLUGIN_DIR/tmux.git.sh</summary>
  
```
#!/bin/sh

session_raw=$(tmux list-sessions -F '#{session_name}:#{session_path}' -f "#{==:#{session_attached},1}")
session_name=$(echo "$session_raw" | cut -d ":" -f 1)
session_path=$(echo "$session_raw" | cut -d ":" -f 2)

if [ "$SENDER" = "tmux_session_update" ]; then
  if [ "$session_name" = "default" ]; then
    sketchybar --set "$NAME" drawing=off 
  else 
    branch=$(git -C "$session_path" rev-parse --abbrev-ref HEAD)
    sketchybar --set "$NAME" drawing=on icon.drawing=on icon=⎇  label="$branch"
  fi
fi
```

</details>


---

Created by [@oschrenk](https://github.com/oschrenk)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-8052246)
