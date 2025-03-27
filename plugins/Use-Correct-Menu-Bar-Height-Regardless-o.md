# Use Correct Menu Bar Height Regardless of Resolution
Not a plugin, instead it's a helper written in Objective-C to get the height in pixels of the menu bar regardless of the resolution.
- This enables you to change resolutions and always have the bar be the correct height.
- Makes your bar usable by anyone regardless of the resolution they use.
- No more hard coding the height of the bar.
- Automatic Yabai window gap setup for different sized bars.

## Code
I put the following code into a file called `get_menu_bar_height.m`.
```obj-c
#import <Cocoa/Cocoa.h>

int main() {
    NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
    NSRect screenRect = [[NSScreen mainScreen] frame];
    NSRect visibleRect = [[NSScreen mainScreen] visibleFrame];

    // Show the menu bar temporarily to get its height
    [NSMenu setMenuBarVisible:YES];

    CGFloat menuBarHeight = screenRect.size.height - visibleRect.size.height;
    printf("%d\n", (int) menuBarHeight);

    // Hide the menu bar again
    [NSMenu setMenuBarVisible:NO];

    [pool drain];
    return 0;
}
```
Compile this on your system using something like the following:
```
clang -framework cocoa get_menu_bar_height.m -o get_menu_bar_height
```
Then you get an executable called `get_menu_bar_height` (feel free to name it what you want).
- Make sure it works by using `./get_menu_bar_height` on the CLI. 

Then you can call it in a bash script (your `sketchybarrc`) and capture its output using command substitution like this:
```bash
height=$("$CONFIG_DIR"/get_menu_bar_height)
```
Or use that value to perform more dynamic calculations of height based on some other parameters like offset or padding by using something like:
```bash
height=$(($("$CONFIG_DIR"/get_menu_bar_height) + PADDING + y_offset))
```
Of course you can make that a lot shorter but this is an example for your understanding.
## Enabling Automatic Resizing using Yabai
If you use Yabai, which I imagine we all do, you can get the bar to resize itself if you're using its Homebrew service.

Add the following to your `yabairc` file:
```bash
yabai -m signal --add event=display_resized action="brew services restart sketchybar"
```
Now, if the resolution changes the bar will automatically resize. Enjoy!

---

Created by [@noncog](https://github.com/noncog)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-6225020)
