### Toggl Current Project and Description
<img width="744" alt="Screenshot 2024-05-07 at 19 41 53" src="https://github.com/FelixKratz/SketchyBar/assets/13283054/a0a5e2eb-a563-43c3-8108-07395772dc09">

**Description:** Uses the Toggl API to query for the current running timer and puts it into your status bar.
**Requirements:** `jq`, `curl`
**Note:** This script requires you to provide your Toggl Username and Password, by default it loads this from `~/.secrets/toggl/username.txt` and `~/.secrets/toggl/password.txt`, However you can simply hardcode in the script if that's what you prefer :)

<details>
  <summary>toggl.sh</summary>

  ```shell
  #!/usr/bin/env zsh
  
  TOGGL_USERNAME=$(cat ~/.secrets/toggl/username.txt)
  TOGGL_PASSWORD=$(cat ~/.secrets/toggl/password.txt)
  
  read ENTRY_DESCRIPTION PROJECT_ID WORKSPACE_ID < <(echo $(curl -s https://api.track.toggl.com/api/v9/me/time_entries/current \
          -H "Content-Type: application/json" \
          -u $TOGGL_USERNAME:$TOGGL_PASSWORD | /jq -r '.description, .pid, .wid'))
  
  if [ $ENTRY_DESCRIPTION = "null" ]; then
          sketchybar --set toggl drawing=off updates=on
          return
  fi
  
  PROJECT_NAME=$(curl -s https://api.track.toggl.com/api/v9/workspaces/$WORKSPACE_ID/projects/$PROJECT_ID \
          -H "Content-Type: application/json" \
          -u $TOGGL_USERNAME:$TOGGL_PASSWORD | jq -r '.name')
  
  sketchybar --set toggl label="$PROJECT_NAME - $ENTRY_DESCRIPTION" drawing=on updates=on
  ```

</details>



<details>
  <summary>sketchybarrc</summary>

  ```
  sketchybar --add item toggl right \
	  --set toggl \
	  icon.color=0xfff48a98 \
	  icon=󰄉 \
	  update_freq=5 \
	  updates=on \
	  drawing=off \
	  script="~/.config/sketchybar/toggl.sh" \
```

</details>

For the `nix` users amongst you, you can see the sketchybarrc [here](https://github.com/JPyke3/Nix-Configuration/blob/45d4b4574f852c7d58b0c76f82de591f4c6af016/programs/daemon/sketchybar/sketchybarrc-laptop#L129) and the shell script [here](https://github.com/JPyke3/Nix-Configuration/blob/45d4b4574f852c7d58b0c76f82de591f4c6af016/programs/daemon/sketchybar/plugins.nix#L501). The Nix config will automatically pull in `jq` and you can load in your secrets from `sops-nix`.

---

Created by [@JPyke3](https://github.com/JPyke3)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-9340065)
