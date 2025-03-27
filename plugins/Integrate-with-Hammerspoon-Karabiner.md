# Integrate with Hammerspoon & Karabiner

## Hammerspoon

<img width="1728" alt="Screenshot 2022-12-25 at 10 55 04 PM" src="https://user-images.githubusercontent.com/26241915/209472745-c652cc54-759a-4d64-9455-0be626da0acb.png">

Changed the colour when triggering [MenuHammer](https://github.com/FryJay/MenuHammer)

```bash
# Example
 hs.execute('/opt/homebrew/bin/sketchybar --animate tanh 20 --bar color=0x6624273a')
```

## Karabiner

<img width="1728" alt="Screenshot 2022-12-25 at 10 55 44 PM" src="https://user-images.githubusercontent.com/26241915/209472764-39faaa3c-16fe-45ca-b68e-e2c684a2cdf2.png">

Changed the colour when triggering [SuperDuper](https://github.com/jasonrudolph/keyboard) for vim movement

```json
# Example
{
    "shell_command": "/opt/homebrew/bin/sketchybar --animate tanh 20 --bar color=0x6624273a"
}
```

It is not really plugin but wanted to share the possibility to executing sketchybar command. Maybe thinking rather changing the bar colour, might have a dot indicator...



---

Created by [@AjayShanker-geek](https://github.com/AjayShanker-geek)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-4493629)
