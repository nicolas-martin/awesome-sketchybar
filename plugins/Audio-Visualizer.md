# Audio Visualizer

![Jun-20-2023 16-22-10](https://github.com/FelixKratz/SketchyBar/assets/12627184/feb3b9c8-66dd-40ff-8075-fd8d7a241803)

## dependencies

- [Background Music](https://github.com/kyleneideck/BackgroundMusic)

```sh
brew install --cask background-music
```

- [cava](https://github.com/karlstav/cava)

```sh
brew install cava
```

## sketchybarrc

```sh
sketchybar --add item cava left               \
           --set cava update_freq=0           \
                 script="$PLUGIN_DIR/cava.sh"
```

## $PLUGIN_DIR/cava.sh

```sh
while true
do
  cava -p $PLUGIN_DIR/cava.conf | sed -u 's/ //g; s/0/▁/g; s/1/▂/g; s/2/▃/g; s/3/▄/g; s/4/▅/g; s/5/▆/g; s/6/▇/g; s/7/█/g; s/8/█/g' | while read line; do
    sketchybar --set $NAME label=$line
  done
  sleep 5
done
```

## $PLUGIN_DIR/cava.conf

```
[general]
framerate=60
bars=10
[input]
method = portaudio
source = "Background Music"
[output]
method=raw
data_format=ascii
ascii_max_range=8
bar_delimiter=32
channels=mono
mono_option=average
```

---

Created by [@ColaMint](https://github.com/ColaMint)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-6224928)
