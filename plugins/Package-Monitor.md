## Package Monitor ##

#### Description ####

A port of the original [Package Monitor](https://github.com/SxC97/Package-Monitor) plugin for BitBar and SwiftBar to keep track of outdated packages using a variety of package managers. Full list of dependencies and supported package managers can be found there.

<img width="68" alt="Screen Shot 2021-10-27 at T20 11 07" src="https://user-images.githubusercontent.com/53032923/139169157-1f15b954-445e-490d-844b-c1fd85dc10ed.png">

#### Known Issues ####

This plugin does not support the full list of features (like displaying the number of outdated packages for each manager) as the original, as sketchybar does not support menu’s. It might be possible to add this feature via the clicking function, but I can only work on that once the core script is in working order.

<details><summary>sketchybarrc</summary><p>

```
sketchybar -m --add item packages right \
              --set packages update_freq=1800 \
              --set packages script="~/.config/sketchybar/plugins/package_monitor.sh" \
              --set packages label=
```

</p></details>

<details><summary>package_monitor.sh</summary><p>

```
# specify the package managers you want the program to use
# valid manager names "brew" "npm" "yarn" "apm" "mas" "pip" and "gem"
USE='brew pip'

# Checks to see if the brew command is avaliable and the package manager is in the enabled list above.
if [[ -x "$(command -v brew)" ]] && [[ $USE == *"brew"* ]]; then

  brew update &> /dev/null

  # runs the outdated command and stores the output as a list variable.
  brewLIST=$(brew outdated)

  # checks to see if the returned list is empty. If so, it sets the outdated packages list to zero, if not, sets it to the line count of the list.
  if [[ $brewLIST == "" ]]; then
    BREW='0'
    brewLIST=""
  else
    BREW=$(echo "$brewLIST" | wc -l)
  fi

fi

# Checks to see if the pip command is avaliable and the package manager is in the enabled list above.
if [[ -x "$(command -v pip3)" ]] && [[ $USE == *"pip3"* ]]; then

  # runs the outdated command and stores the output as a list variable.
  pipLIST=$(pip3 list --outdated)
  tempPIP=$(echo "$pipLIST" | wc -l)

  # checks to see if the returned list is empty. If so, it sets the outdated packages list to zero, if not, sets it to the line count of the list.
  if [[ $tempPIP -gt 1 ]]; then
    PIP=$(($tempPIP - 2))
  else
    PIP="0"
    pipLIST=""
  fi

fi

# Checks to see if the npm command is avaliable and the package manager is in the enabled list above.
if [[ -x "$(command -v npm)" ]] && [[ $USE == *"npm"* ]]; then

  npm update &> /dev/null

  # runs the outdated command and stores the output as a list variable.
  npmLIST=$(npm outdated)

  # checks to see if the returned list is empty. If so, it sets the outdated packages list to zero, if not, sets it to the line count of the list.
  if [[ $npmLIST == "" ]]; then
    NPM='0'
    npmLIST=""
  else
    NPM=$(echo "npmLIST" | wc -l)
  fi

fi

# Checks to see if the yarn command is avaliable and the package manager is in the enabled list above.
if [[ -x "$(command -v yarn)" ]] && [[ $USE == *"yarn"* ]]; then

  # runs the outdated command and stores the output as a list variable.
  yarnLIST=$(yarn outdated)

  # checks to see if the returned list is empty. If so, it sets the outdated packages list to zero, if not, sets it to the line count of the list.
  if [[ $yarnLIST == "" ]]; then
    YARN='0'
    yarnLIST=""
  else
    YARN=$(echo "$yarnLIST" | wc -l)
  fi

fi

# Checks to see if the apm command is avaliable and the package manager is in the enabled list above.
if [[ -x "$(command -v apm)" ]] && [[ $USE == *"apm"* ]]; then

  apm update &> /dev/null

  # runs the outdated command and stores the output as a list variable.
  apmLIST="$(apm outdated)"

  # checks to see if the returned list is empty. If so, it sets the outdated packages list to zero, if not, sets it to the line count of the list.
  if [[ $apmLIST == *"empty"* ]]; then
    APM='0'
    apmLIST=""
  else
  tempAPM=$(echo "$apmLIST" | wc -l)
  APM=$((tempAPM - 1))
  fi

fi

# Checks to see if the gem command is avaliable and the package manager is in the enabled list above.
if [[ -x "$(command -v gem)" ]] && [[ $USE == *"gem"* ]]; then

  # runs the outdated command and stores the output as a list variable.
  gemLIST=$(gem outdated)

  # checks to see if the returned list is empty. If so, it sets the outdated packages list to zero, if not, sets it to the line count of the list.
  if [[ $gemLIST == "" ]]; then
    GEM='0'
    gemLIST=""
  else
    GEM=$(echo "$gemLIST" | wc -l)
  fi

fi

# Checks to see if the mas command is avaliable and the package manager is in the enabled list above.
if [[ -x "$(command -v mas)" ]] && [[ $USE == *"mas"* ]]; then

  # runs the outdated command and stores the output as a list variable.
  masLIST=$(mas outdated)

  # checks to see if the returned list is empty. If so, it sets the outdated packages list to zero, if not, sets it to the line count of the list.
  if [[ $masLIST == "" ]]; then
    MAS='0'
    masLIST=""
  else
    MAS=$(echo "$masLIST" | wc -l)
  fi

fi

DEFAULT="0"

# sum of all outdated packages
SUM=$(( ${BREW:-DEFAULT} + ${CASK:-DEFAULT} + ${PIP:-DEFAULT} + ${NPM:-DEFAULT} + ${YARN:-DEFAULT} + ${APM:-DEFAULT} + ${GEM:-DEFAULT} + ${MAS:-DEFAULT} ))

# icon to be displayed next to number of outdated packages. Feel free to customize. Default: 
ICON=""

# icon to be displayed if no packages are outdated. Change to `ZERO=""` if you want the widget to be invisible when no packages are out of date. Default: ✔︎
ZERO="✔︎"

if [[ $SUM -gt 0 ]]; then
  FINAL="$SUM $ICON"
else
  FINAL="$ZERO"
fi

sketchybar -m --set packages label="$FINAL"
```

</p></details>

---

Created by [@SxC97](https://github.com/SxC97)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-1549440)
