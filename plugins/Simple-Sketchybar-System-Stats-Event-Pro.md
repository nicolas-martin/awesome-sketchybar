# Simple Sketchybar System Stats Event Provider

![System stats](https://github.com/user-attachments/assets/14759cf4-161f-4958-900f-1b871001f44a)

I was interested in learning Rust, so I decided to try my hand at creating a small Rust helper application to send trigger messages to Sketchybar to update system stats.

https://github.com/joncrangle/sketchybar-system-stats

It is a small CLI app that you can invoke from your `sketchybarrc` or `SbarLua` module. You pass args to the cli for the system stats you're interested in when you launch it, subscribe your items to the `system_stats` event, and use the ENV vars to update item icons or labels. My goal was to keep it small, lightweight and simple.

Currently, the app can send trigger events for:
* system (arch, distro, host_name, kernel_version, name, os_version, long_os_version, uptime)
* cpu (count, frequency, temperature, usage)
* disk (count, free, total, usage, used)
* memory (ram_available, ram_total, ram_usage, ram_used, swp_free, swp_total, swp_usage, swp_used)
* network (rx/tx in KB/s for interfaces)

You can also specify a desired update interval (default 5 seconds) or bar name (if not using default). You can see all the options with `stats_provider --help` or by checking out the `README`.

This is my first ever Rust project, and it's been a good learning experience so far. If you're interested in trying it, you can clone and build it from source, or download the prebuilt binary for your Mac architecture. 

Example usage to create a disk_usage item:
<details>
<summary>sketchybarrc</summary>

```bash
## Start the helper with desired CLI args
killall stats_provider
# Update with path to stats_provider and invoke cli for your desired stats
# This example will send cpu, disk and ram usage percentages
$CONFIG_DIR/sketchybar-system-stats/target/release/stats_provider --cpu usage --disk usage --memory ram_usage &
sketchybar --add item disk_usage right \
           --set disk_usage script="sketchybar --set disk_usage label=\$DISK_USAGE" \
           --subscribe disk_usage system_stats
```
</details>
<details>
<summary>disk_stats.lua</summary>

```lua
-- Update with path to stats_provider
sbar.exec('killall stats_provider >/dev/null; $CONFIG_DIR/sketchybar-system-stats/target/release/stats_provider --cpu usage --disk usage --memory usage')
-- Subscribe and use the `DISK_USAGE` var

local disk_usage = sbar.add('item', 'disk_usage', {
	position = 'right',
})

disk_usage:subscribe('system_stats', function(env)
	disk_usage:set { label = env.DISK_USAGE }
end)
```
</details>

You can create other items following the same pattern and style them to your heart's content.

---

Created by [@joncrangle](https://github.com/joncrangle)

[View Original Discussion](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-10468971)
