
# Awesome SketchyBar Plugins

A community‑curated list of SketchyBar plugins originally shared in [SketchyBar discussion #12](https://github.com/FelixKratz/SketchyBar/discussions/12).  
Each plugin entry includes a short description, the original discussion link, and a pointer to the plugin’s dedicated code file in the `./plugins/` folder.

> **Note:** This `README.md` only references each plugin. The actual plugin code (configuration snippets, scripts) should live in separate Markdown files under `./plugins/`. We’ll generate those files later.

---

## Table of Contents

- [Window Title](#window-title)
- [24-Hour Bitcoin Price](#24-hour-bitcoin-price)
- [24-Hour Ethereum Price](#24-hour-ethereum-price)
- [Battery Status](#battery-status)
- [Music Information](#music-information)
- [VPN Status](#vpn-status)
- [Mic Status](#mic-status)
- [Reminders](#reminders)
- [MPD](#mpd)
- [Stack Indicator](#stack-indicator)
- [SKHD Modal Indicator](#skhd-modal-indicator)
- [IP Plugin](#ip-plugin)
- [Transmission Speeds](#transmission-speeds)
- [Disk Capacity Monitor](#disk-capacity-monitor)
- [Taskwarrior Popup](#taskwarrior-popup)
- [Contributing](#contributing)
- [License](#license)

---

### Window Title
**Description:** Displays the current window’s title (fallback to app name) with truncation for long titles. Triggered by Yabai signals.  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-476955)  
**Code:** [plugins/window-title.md](plugins/window-title.md)  
**Author(s):** [@FelixKratz](https://github.com/FelixKratz), [@typkrft](https://github.com/typkrft)

---

### 24-Hour Bitcoin Price
**Description:** Fetches BTC’s 24-hour price change from Gemini’s API and displays the percent.  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-476956)  
**Code:** [plugins/bitcoin-price.md](plugins/bitcoin-price.md)  
**Author:** [@typkrft](https://github.com/typkrft)

---

### 24-Hour Ethereum Price
**Description:** Similar to the Bitcoin plugin, but for ETH’s 24-hour price change.  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-476957)  
**Code:** [plugins/ethereum-price.md](plugins/ethereum-price.md)  
**Author:** [@typkrft](https://github.com/typkrft)

---

### Battery Status
**Description:** Displays the current battery percentage, charging status, and relevant icons.  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-476958)  
**Code:** [plugins/battery-status.md](plugins/battery-status.md)  
**Author(s):** [@typkrft](https://github.com/typkrft), [@ut0mt8](https://github.com/ut0mt8)

---

### Music Information
**Description:** Shows track info (title, artist) from the Music.app and updates on song changes.  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-476959)  
**Code:** [plugins/music-info.md](plugins/music-info.md)  
**Author:** [@typkrft](https://github.com/typkrft)

---

### VPN Status
**Description:** Detects if you’re connected to a VPN and displays the VPN name.  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-476960)  
**Code:** [plugins/vpn-status.md](plugins/vpn-status.md)  
**Author(s):** [@typkrft](https://github.com/typkrft), [@Pe8er](https://github.com/Pe8er)

---

### Mic Status
**Description:** Toggles mic mute/unmute on click; icon changes to reflect mic status.  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-476961)  
**Code:** [plugins/mic-status.md](plugins/mic-status.md)  
**Author(s):** [@typkrft](https://github.com/typkrft), [@dtger](https://github.com/dtger), [@linkarzu](https://github.com/linkarzu)

---

### Reminders
**Description:** Displays the count of pending Apple Reminders and opens the app on click.  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-476962)  
**Code:** [plugins/reminders.md](plugins/reminders.md)  
**Author:** [@typkrft](https://github.com/typkrft)

---

### MPD
**Description:** Retrieves and displays current MPD track information and playback state.  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-476963)  
**Code:** [plugins/mpd.md](plugins/mpd.md)  
**Author(s):** [@johnallen3d](https://github.com/johnallen3d), [@rhyses-pieces](https://github.com/rhyses-pieces)

---

### Stack Indicator
**Description:** Provides a dot or numeric indicator for yabai’s stack of windows.  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-476964)  
**Code:** [plugins/stack-indicator.md](plugins/stack-indicator.md)  
**Author:** [@typkrft](https://github.com/typkrft)

---

### SKHD Modal Indicator
**Description:** Shows which skhd mode is active (normal, window, scripts, etc.).  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-476965)  
**Code:** [plugins/skhd-modal.md](plugins/skhd-modal.md)  
**Author:** [@typkrft](https://github.com/typkrft)

---

### IP Plugin
**Description:** Displays the IP address of a specified interface (e.g. `en0`).  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-4928028)  
**Code:** [plugins/ip-plugin.md](plugins/ip-plugin.md)  
**Author:** [@sdpathak24](https://github.com/sdpathak24)

---

### Transmission Speeds
**Description:** Monitors download/upload speeds via `transmission-remote`.  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-4928609)  
**Code:** [plugins/transmission.md](plugins/transmission.md)  
**Author:** [@Pe8er](https://github.com/Pe8er)

---

### Disk Capacity Monitor
**Description:** Displays system volume usage; click toggles the label on/off.  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-4975278)  
**Code:** [plugins/disk-monitor.md](plugins/disk-monitor.md)  
**Author:** [@Pe8er](https://github.com/Pe8er)

---

### Taskwarrior Popup
**Description:** Shows pending Taskwarrior tasks in a popup; overdue tasks in red.  
**Original Discussion:** [Link](https://github.com/FelixKratz/SketchyBar/discussions/12#discussioncomment-5011141)  
**Code:** [plugins/taskwarrior.md](plugins/taskwarrior.md)  
**Author:** [@EphraimSiegfried](https://github.com/EphraimSiegfried)

---

## Contributing

1. **Fork** this repository.  
2. Add a `.md` file in `plugins/` for your plugin.  
3. In this `README.md`, add a new entry with:
   - Plugin title
   - Short description
   - Link to the original discussion
   - Link to your new `.md` file
   - Credit authors
4. Open a **Pull Request**.

## License

[MIT License](LICENSE)

