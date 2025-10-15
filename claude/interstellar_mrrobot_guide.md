# The Ultimate Interstellar & Mr. Robot Ricing Guide for Arch Linux

## Philosophy & Approach

Before we dive into commands and configurations, let's understand what makes each theme special and how to approach building them.

### Interstellar: Cooper & TARS Theme Philosophy

The Interstellar theme is about creating a sense of vastness, precision, and futuristic minimalism. Think about the movie's aesthetic: clean spacecraft interfaces, the stark beauty of space, time dilation visualizations, and TARS's geometric, utilitarian design. Your desktop should feel like a command center on the Endurance spacecraft—functional, beautiful, and awe-inspiring.

**Core Visual Principles:**
- **Color Palette:** Deep blacks representing space, warm oranges and golds from the wormhole and Gargantua scenes, cool blues from ice planets and space, with crisp whites for text and UI elements
- **Typography:** Clean, geometric fonts that feel technical but not cluttered. Think NASA mission control displays
- **Animations:** Smooth, physics-based transitions that respect momentum and gravity, nothing jarring
- **Minimalism:** Every element serves a purpose, like TARS's honest percentage readings
- **Geometry:** Rectangular, modular designs inspired by TARS's transformable body

### Mr. Robot: Elliot Theme Philosophy

Mr. Robot is raw, gritty, and authentic. This theme embodies the hacker aesthetic—terminal-first workflow, monospace fonts everywhere, minimal graphics, green-on-black terminals with glitch effects, and an overall paranoid, surveillance-aware vibe. Your desktop should feel like fsociety's hideout or Elliot's apartment: utilitarian, focused, with easter eggs hidden throughout.

**Core Visual Principles:**
- **Color Palette:** Deep blacks, terminal greens and reds, occasional purple/magenta for accent, desaturated grays
- **Typography:** Monospace everything—Hack font or Source Code Pro, representing code-first thinking
- **Effects:** Subtle glitch effects, scanlines, CRT-like distortions, terminal aesthetic
- **Workflow:** Heavy terminal usage, minimal GUI elements, keyboard-driven everything
- **Theming:** Dark, minimal, with occasional intentional "bugs" or glitches as design elements

---

## Phase 1: Foundation & Core Decisions

### Display Server Choice

For both themes, I recommend **Wayland** specifically using **Hyprland** as your compositor. Here's why this makes sense:

**For Interstellar:** Hyprland offers incredibly smooth animations that can mimic the fluid camera movements in the film. The blur effects work beautifully for creating depth, and the modern architecture feels appropriately futuristic. You can create animations that feel like floating through space or the time-warping effects near Gargantua.

**For Mr. Robot:** While Mr. Robot has a retro-tech aesthetic, Hyprland's performance means you can layer multiple terminals and still maintain smooth performance. The keyboard-driven workflow fits perfectly with Elliot's character, and the tiling nature means maximum screen real estate for terminals.

**Installation:**
```bash
sudo pacman -S hyprland xdg-desktop-portal-hyprland
```

**Alternative for X11 users:** If you prefer staying on X11, use **i3-gaps** with **picom** (jonaburg fork for animations). The configuration will be similar but you'll need to adjust compositor settings.

### AUR Helper Setup

Before we go further, install an AUR helper. I recommend **paru** for its speed and modern Rust architecture:

```bash
sudo pacman -S --needed base-devel git
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
cd .. && rm -rf paru
```

---

## Phase 2: Interstellar Theme - "Endurance" Build

### Step 1: Color Scheme Foundation

The Interstellar palette draws from the film's most iconic scenes. Let's establish our base colors:

**Primary Colors:**
- **Space Black:** `#0a0e14` (deep space background)
- **Gargantua Orange:** `#ff9933` (warm accents, representing the wormhole glow)
- **Tesseract Gold:** `#ffd700` (highlights and important elements)
- **Ice Blue:** `#4d9de0` (secondary accent from Miller's planet)
- **Endurance White:** `#e6e6e6` (primary text)
- **Panel Gray:** `#1a1f29` (UI backgrounds, slightly lighter than space black)

Create a file at `~/.config/colors/interstellar.conf` to store these values:

```conf
# Interstellar Color Scheme
background=#0a0e14
foreground=#e6e6e6
accent_primary=#ff9933
accent_secondary=#4d9de0
accent_tertiary=#ffd700
panel_bg=#1a1f29
urgent=#e74c3c
```

### Step 2: Hyprland Configuration

Hyprland will be your window manager and compositor. The configuration file goes in `~/.config/hypr/hyprland.conf`. Let me break down the essential sections:

**Monitor Setup:**
```conf
# Configure your monitor(s) - adjust resolution as needed
monitor=,preferred,auto,1

# If you have multiple monitors, you can set them up like:
# monitor=DP-1,1920x1080@144,0x0,1
# monitor=HDMI-A-1,1920x1080@60,1920x0,1
```

**Input Configuration:**
```conf
input {
    kb_layout = us
    follow_mouse = 1
    sensitivity = 0
    
    touchpad {
        natural_scroll = true
        disable_while_typing = true
    }
}
```

**Aesthetics - This is where the magic happens:**
```conf
general {
    gaps_in = 8  # Inner gaps between windows
    gaps_out = 16  # Outer gaps to screen edges
    border_size = 2
    col.active_border = rgba(ff9933ee) rgba(4d9de0ee) 45deg  # Gradient border
    col.inactive_border = rgba(1a1f29aa)
    
    layout = dwindle  # Dwindle layout is clean and predictable
}

decoration {
    rounding = 12  # Rounded corners for modern look
    
    blur {
        enabled = true
        size = 8
        passes = 3
        new_optimizations = true
        ignore_opacity = true
        xray = true  # Don't blur floating windows over tiled ones
    }
    
    drop_shadow = true
    shadow_range = 20
    shadow_render_power = 3
    col.shadow = rgba(0a0e1499)
    
    dim_inactive = true
    dim_strength = 0.1  # Subtle dimming for inactive windows
}

animations {
    enabled = true
    
    # These animation curves feel like floating in zero gravity
    bezier = smoothOut, 0.36, 0, 0.66, -0.56
    bezier = smoothIn, 0.25, 1, 0.5, 1
    bezier = overshot, 0.13, 0.99, 0.29, 1.1
    
    animation = windows, 1, 5, overshot, slide
    animation = windowsOut, 1, 4, smoothOut, slide
    animation = border, 1, 10, default
    animation = fade, 1, 8, smoothIn
    animation = workspaces, 1, 6, overshot, slidevert
}
```

**Workspace Configuration:**
```conf
# Name your workspaces after locations in the film
workspace = 1, name:Endurance
workspace = 2, name:Miller
workspace = 3, name:Mann
workspace = 4, name:Cooper
workspace = 5, name:Gargantua
```

**Keybindings - Keep it logical and comfortable:**
```conf
$mainMod = SUPER  # Use Windows/Super key

# Essential window management
bind = $mainMod, Q, killactive
bind = $mainMod, M, exit
bind = $mainMod, V, togglefloating
bind = $mainMod, F, fullscreen
bind = $mainMod, P, pseudo  # dwindle pseudo-tiling

# Launch applications
bind = $mainMod, Return, exec, kitty
bind = $mainMod, D, exec, rofi -show drun
bind = $mainMod, E, exec, thunar

# Move focus with vim keys
bind = $mainMod, H, movefocus, l
bind = $mainMod, L, movefocus, r
bind = $mainMod, K, movefocus, u
bind = $mainMod, J, movefocus, d

# Switch workspaces
bind = $mainMod, 1, workspace, 1
bind = $mainMod, 2, workspace, 2
bind = $mainMod, 3, workspace, 3
bind = $mainMod, 4, workspace, 4
bind = $mainMod, 5, workspace, 5

# Move active window to workspace
bind = $mainMod SHIFT, 1, movetoworkspace, 1
bind = $mainMod SHIFT, 2, movetoworkspace, 2
bind = $mainMod SHIFT, 3, movetoworkspace, 3
bind = $mainMod SHIFT, 4, movetoworkspace, 4
bind = $mainMod SHIFT, 5, movetoworkspace, 5

# Mouse bindings
bindm = $mainMod, mouse:272, movewindow
bindm = $mainMod, mouse:273, resizewindow

# Screenshot with grim + slurp
bind = , Print, exec, grim -g "$(slurp)" - | wl-copy
bind = SHIFT, Print, exec, grim ~/Pictures/screenshot-$(date +%Y%m%d-%H%M%S).png
```

**Install required tools:**
```bash
paru -S hyprland kitty rofi-wayland thunar grim slurp wl-clipboard
```

### Step 3: Wallpaper - The Cosmic Background

For Interstellar, your wallpaper is crucial. You want high-resolution space imagery or stills from the film. I recommend using **swww** for wallpaper management on Wayland.

```bash
paru -S swww
```

**Setup swww to start with Hyprland:**

Add to your `hyprland.conf`:
```conf
exec-once = swww init
exec-once = swww img ~/Pictures/Wallpapers/interstellar-gargantua.jpg --transition-type fade --transition-duration 2
```

**Where to find wallpapers:**
- Search for "Interstellar 4K wallpaper" on wallpaperflare.com or wallhaven.cc
- Look for images of: Gargantua black hole, the wormhole, Saturn, the Endurance spacecraft, tesseract scenes
- Aim for dark images with prominent blacks to maintain the space aesthetic

**Dynamic wallpaper script (optional but cool):**

Create `~/.config/hypr/scripts/wallpaper-rotate.sh`:
```bash
#!/bin/bash
WALLPAPER_DIR=~/Pictures/Wallpapers/Interstellar

while true; do
    # Pick random wallpaper
    WALL=$(find "$WALLPAPER_DIR" -type f | shuf -n 1)
    swww img "$WALL" --transition-type fade --transition-duration 3
    
    # Change every 30 minutes
    sleep 1800
done
```

Make it executable and add to Hyprland autostart:
```bash
chmod +x ~/.config/hypr/scripts/wallpaper-rotate.sh
```

Add to `hyprland.conf`:
```conf
exec-once = ~/.config/hypr/scripts/wallpaper-rotate.sh
```

### Step 4: Status Bar - Waybar Configuration

Waybar is your mission control display. Think of it like the HUD displays inside the Endurance. Install and configure:

```bash
paru -S waybar otf-font-awesome ttf-jetbrains-mono-nerd
```

Create `~/.config/waybar/config`:
```json
{
    "layer": "top",
    "position": "top",
    "height": 34,
    "spacing": 4,
    
    "modules-left": ["hyprland/workspaces", "hyprland/window"],
    "modules-center": ["clock"],
    "modules-right": ["pulseaudio", "network", "cpu", "memory", "battery", "tray"],
    
    "hyprland/workspaces": {
        "disable-scroll": false,
        "all-outputs": true,
        "format": "{name}",
        "on-click": "activate"
    },
    
    "hyprland/window": {
        "format": "{}",
        "max-length": 50,
        "separate-outputs": true
    },
    
    "clock": {
        "timezone": "UTC",
        "tooltip-format": "<big>{:%Y %B}</big>\n<tt><small>{calendar}</small></tt>",
        "format": "{:%H:%M:%S UTC}",
        "interval": 1
    },
    
    "cpu": {
        "format": " {usage}%",
        "tooltip": true,
        "interval": 2
    },
    
    "memory": {
        "format": " {}%",
        "tooltip-format": "{used:0.1f}G / {total:0.1f}G used",
        "interval": 2
    },
    
    "battery": {
        "states": {
            "warning": 30,
            "critical": 15
        },
        "format": "{icon} {capacity}%",
        "format-charging": " {capacity}%",
        "format-icons": ["", "", "", "", ""]
    },
    
    "network": {
        "format-wifi": " {signalStrength}%",
        "format-ethernet": " Connected",
        "format-disconnected": "⚠ Disconnected",
        "tooltip-format": "{ifname}: {ipaddr}/{cidr}"
    },
    
    "pulseaudio": {
        "format": "{icon} {volume}%",
        "format-muted": " Muted",
        "format-icons": {
            "default": ["", "", ""]
        },
        "on-click": "pavucontrol"
    }
}
```

Create `~/.config/waybar/style.css`:
```css
* {
    font-family: "JetBrains Mono Nerd Font", monospace;
    font-size: 13px;
    font-weight: 500;
}

window#waybar {
    background-color: rgba(26, 31, 41, 0.9);
    color: #e6e6e6;
    border-bottom: 2px solid #ff9933;
}

#workspaces button {
    padding: 0 12px;
    color: #e6e6e6;
    background-color: transparent;
    border: none;
    border-radius: 0;
}

#workspaces button.active {
    background-color: #ff9933;
    color: #0a0e14;
    font-weight: bold;
}

#workspaces button:hover {
    background-color: rgba(77, 157, 224, 0.3);
}

#window {
    margin: 0 12px;
    color: #ffd700;
}

#clock {
    color: #4d9de0;
    font-weight: bold;
    padding: 0 16px;
}

#cpu, #memory, #battery, #network, #pulseaudio {
    padding: 0 12px;
}

#cpu {
    color: #ff9933;
}

#memory {
    color: #ffd700;
}

#battery {
    color: #4d9de0;
}

#battery.warning {
    color: #f39c12;
}

#battery.critical {
    color: #e74c3c;
    animation: blink 1s linear infinite;
}

@keyframes blink {
    50% { opacity: 0.5; }
}

#network {
    color: #e6e6e6;
}

#pulseaudio {
    color: #ff9933;
}

#tray {
    padding: 0 12px;
}
```

Add to `hyprland.conf`:
```conf
exec-once = waybar
```

### Step 5: Application Launcher - Rofi

Rofi is your quick access menu, like TARS responding to commands. Configure it for a sleek, space-themed look.

Create `~/.config/rofi/config.rasi`:
```css
configuration {
    modi: "drun,run,window";
    show-icons: true;
    drun-display-format: "{name}";
    font: "JetBrains Mono 12";
    display-drun: " Launch";
    display-window: " Window";
}

@theme "interstellar"
```

Create `~/.config/rofi/interstellar.rasi`:
```css
* {
    bg: #0a0e14e6;
    fg: #e6e6e6;
    accent: #ff9933;
    secondary: #4d9de0;
    urgent: #e74c3c;
    
    background-color: transparent;
    text-color: @fg;
}

window {
    background-color: @bg;
    border: 2px solid;
    border-color: @accent;
    border-radius: 12px;
    padding: 20px;
    width: 600px;
}

mainbox {
    spacing: 20px;
    children: [inputbar, listview];
}

inputbar {
    background-color: #1a1f29;
    border-radius: 8px;
    padding: 12px 16px;
    spacing: 12px;
    children: [prompt, entry];
}

prompt {
    text-color: @accent;
    font: "JetBrains Mono Bold 13";
}

entry {
    placeholder: "Search...";
    placeholder-color: #666666;
}

listview {
    lines: 8;
    spacing: 6px;
}

element {
    padding: 10px 14px;
    border-radius: 8px;
}

element selected {
    background-color: @accent;
    text-color: #0a0e14;
    font: "JetBrains Mono Bold 12";
}

element-icon {
    size: 24px;
    margin: 0 10px 0 0;
}
```

### Step 6: Terminal - Kitty Configuration

Kitty is fast, GPU-accelerated, and perfect for our space theme. Configure it at `~/.config/kitty/kitty.conf`:

```conf
# Font configuration
font_family      JetBrains Mono Nerd Font
bold_font        JetBrains Mono Nerd Font Bold
italic_font      JetBrains Mono Nerd Font Italic
font_size 11.0

# Cursor
cursor_shape beam
cursor_beam_thickness 1.5
cursor_blink_interval 0

# Window appearance
background_opacity 0.92
window_padding_width 12
confirm_os_window_close 0

# Tab bar
tab_bar_edge top
tab_bar_style fade
tab_fade 0.5 1 1 1
active_tab_foreground   #0a0e14
active_tab_background   #ff9933
inactive_tab_foreground #e6e6e6
inactive_tab_background #1a1f29

# Interstellar color scheme
foreground #e6e6e6
background #0a0e14

# Black
color0 #1a1f29
color8 #4a5568

# Red
color1 #e74c3c
color9 #ec7063

# Green
color2  #2ecc71
color10 #58d68d

# Yellow
color3  #ffd700
color11 #f4d03f

# Blue
color4  #4d9de0
color12 #5dade2

# Magenta
color5  #9b59b6
color13 #af7ac5

# Cyan
color6  #1abc9c
color14 #48c9b0

# White
color7  #e6e6e6
color15 #ffffff

# Orange (accent)
color16 #ff9933
color17 #f39c12
```

### Step 7: Shell Customization - Zsh with Starship

Install and configure a beautiful shell prompt:

```bash
paru -S zsh starship
chsh -s /bin/zsh
```

Create `~/.config/starship.toml`:
```toml
format = """
[╭─](bold #ff9933)$username$hostname$directory$git_branch$git_status
[╰─](bold #ff9933)$character"""

[username]
show_always = true
format = "[$user]($style)@"
style_user = "bold #4d9de0"

[hostname]
ssh_only = false
format = "[$hostname]($style) "
style = "bold #ffd700"

[directory]
format = "[$path]($style)[$read_only]($read_only_style) "
style = "bold #e6e6e6"
truncation_length = 3
truncate_to_repo = true

[git_branch]
format = "[$symbol$branch]($style) "
style = "bold #ff9933"
symbol = " "

[git_status]
format = "([\\[$all_status$ahead_behind\\]]($style) )"
style = "bold #e74c3c"

[character]
success_symbol = "[➜](bold #2ecc71)"
error_symbol = "[➜](bold #e74c3c)"

[cmd_duration]
min_time = 500
format = "took [$duration]($style) "
style = "bold #ffd700"
```

Configure `~/.zshrc`:
```bash
# Starship prompt
eval "$(starship init zsh)"

# Aliases
alias ls='exa --icons --group-directories-first'
alias ll='exa -l --icons --group-directories-first'
alias la='exa -la --icons --group-directories-first'
alias cat='bat'
alias vim='nvim'

# History
HISTSIZE=10000
SAVEHIST=10000
HISTFILE=~/.zsh_history

# Auto-completion
autoload -Uz compinit && compinit

# Syntax highlighting and suggestions
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
```

Install plugins:
```bash
paru -S zsh-syntax-highlighting zsh-autosuggestions exa bat
```

### Step 8: Notifications - Dunst

Configure dunst at `~/.config/dunst/dunstrc`:

```ini
[global]
    monitor = 0
    follow = mouse
    width = 350
    origin = top-right
    offset = 16x48
    progress_bar = true
    progress_bar_height = 8
    progress_bar_frame_width = 1
    progress_bar_min_width = 150
    progress_bar_max_width = 350
    
    notification_limit = 3
    separator_height = 2
    padding = 16
    horizontal_padding = 16
    frame_width = 2
    frame_color = "#ff9933"
    separator_color = frame
    
    font = JetBrains Mono 11
    line_height = 4
    format = "<b>%s</b>\n%b"
    alignment = left
    show_age_threshold = 60
    word_wrap = yes
    
    corner_radius = 12
    
[urgency_low]
    background = "#1a1f29e6"
    foreground = "#e6e6e6"
    timeout = 5

[urgency_normal]
    background = "#1a1f29e6"
    foreground = "#e6e6e6"
    frame_color = "#4d9de0"
    timeout = 8

[urgency_critical]
    background = "#1a1f29e6"
    foreground = "#ffffff"
    frame_color = "#e74c3c"
    timeout = 0
```

Add to `hyprland.conf`:
```conf
exec-once = dunst
```

---

## Phase 3: Mr. Robot Theme - "fsociety" Build

Now let's build the Mr. Robot theme. I'll focus on the key differences from Interstellar, as the foundation remains similar.

### Step 1: Color Scheme - The Hacker Palette

Mr. Robot uses harsh, high-contrast colors. Create `~/.config/colors/mrrobot.conf`:

```conf
# Mr. Robot Color Scheme
background=#0d0208
foreground=#00ff41
accent_primary=#ff0055
accent_secondary=#a663cc
accent_tertiary=#00ff41
panel_bg=#1a1a1a
urgent=#ff0055
terminal_green=#00ff41
```

### Step 2: Hyprland Configuration Adjustments

Modify your `hyprland.conf` for a more aggressive, minimal look:

```conf
general {
    gaps_in = 4
    gaps_out = 8
    border_size = 2
    col.active_border = rgba(00ff41ee)
    col.inactive_border = rgba(1a1a1aaa)
    
    layout = dwindle
}

decoration {
    rounding = 0  # No rounded corners - harsh rectangles
    
    blur {
        enabled = false  # No blur - raw and unfiltered
    }
    
    drop_shadow = false  # No shadows
    dim_inactive = false
}

animations {
    enabled = true
    
    # Snappy, quick animations - no smoothness
    bezier = snap, 0.1, 0.8, 0.2, 1.0
    
    animation = windows, 1, 3, snap, slide
    animation = windowsOut, 1, 2, snap, slide
    animation = fade, 1, 4, default
    animation = workspaces, 1, 3, snap, slide
}

# Workspace names from the show
workspace = 1, name:fsociety
workspace = 2, name:allsafe
workspace = 3, name:ecorp
workspace = 4, name:root
workspace = 5, name:daemon
```

### Step 3: Waybar - Terminal Aesthetic

Create `~/.config/waybar/mrrobot-config`:
```json
{
    "layer": "top",
    "position": "top",
    "height": 28,
    "spacing": 0,
    
    "modules-left": ["hyprland/workspaces"],
    "modules-center": ["hyprland/window"],
    "modules-right": ["network", "cpu", "memory", "clock"],
    
    "hyprland/workspaces": {
        "format": "[{name}]",
        "on-click": "activate"
    },
    
    "clock": {
        "format": "[{:%H:%M:%S}]",
        "interval": 1
    },
    
    "cpu": {
        "format": "[CPU:{usage}%]",
        "interval": 2
    },
    
    "memory": {
        "format": "[MEM:{percentage}%]",
        "interval": 2
    },
    
    "network": {
        "format-wifi": "[WIFI:{signalStrength}%]",
        "format-ethernet": "[ETH:UP]",
        "format-disconnected": "[NET:DOWN]"
    }
}
```

Create `~/.config/waybar/mrrobot-style.css`:
```css
* {
    font-family: "Hack Nerd Font", "Source Code Pro", monospace;
    font-size: 11px;
    font-weight: bold;
}

window#waybar {
    background-color: #0d0208;
    color: #00ff41;
    border-bottom: 1px solid #00ff41;
}

#workspaces button {
    padding: 0 8px;
    color: #00ff41;
    background-color: transparent;
    border-right: 1px solid #1a1a1a;
}

#workspaces button.active {
    background-color: #00ff41;
    color: #0d0208;
}

#workspaces button:hover {
    background-color: #1a1a1a;
}

#window {
    color: #ffffff;
    padding: 0 12px;
}

#clock, #cpu, #memory, #network {
    padding: 0 8px;
    border-left: 1px solid #1a1a1a;
}

#clock {
    color: #ff0055;
}

#cpu {
    color: #00ff41;
}

#memory {
    color: #a663cc;
}

#network {
    color: #00ff41;
}
```

### Step 4: Terminal - Kitty Hacker Mode

Create `~/.config/kitty/mrrobot.conf`:

```conf
# Font
font_family      Hack Nerd Font Mono
bold_font        Hack Nerd Font Mono Bold
font_size 10.0

# Cursor
cursor_shape block
cursor_blink_interval 0.5

# Window
background_opacity 0.95
window_padding_width 8

# Mr. Robot color scheme
foreground #00ff41
background #0d0208
cursor #00ff41
selection_foreground #0d0208
selection_background #00ff41

# Black
color0 #0d0208
color8 #1a1a1a

# Red
color1 #ff0055
color9 #ff4081

# Green (terminal green)
color2  #00ff41
color10 #00ff41

# Yellow
color3  #ffff00
color11 #ffff00

# Blue
color4  #0080ff
color12 #40a0ff

# Magenta
color5  #a663cc
color13 #ba7ddf

# Cyan
color6  #00ffff
color14 #40ffff

# White
color7  #ffffff
color15 #ffffff

# Tab bar
active_tab_foreground   #0d0208
active_tab_background   #00ff41
inactive_tab_foreground #00ff41
inactive_tab_background #1a1a1a
```

To switch between themes, modify your kitty command in `hyprland.conf`:
```conf
bind = $mainMod, Return, exec, kitty --config ~/.config/kitty/mrrobot.conf
```

### Step 5: Additional Mr. Robot Touches

**Install CMatrix for background terminals:**
```bash
paru -S cmatrix
```

Add to `hyprland.conf` for a cool startup effect:
```conf
exec-once = kitty --title "MATRIX" -e cmatrix -ab -C magenta
```

**Install Hollywood for fake hacker screens:**
```bash
paru -S hollywood
```

**Neofetch customization** - create `~/.config/neofetch/config.conf`:
```bash
print_info() {
    prin "┌─────────\n Hardware Information \n─────────┐"
    info "OS" distro
    info "Host" model
    info "Kernel" kernel
    info "Uptime" uptime
    info "Packages" packages
    info "Shell" shell
    info "Terminal" term
    prin "└───────────────────────────────────────┘"
    prin "┌─────────\n Network Information \n──────────┐"
    info "Local IP" local_ip
    prin "└───────────────────────────────────────┘"
}

# ASCII art
ascii_distro="arch_small"
ascii_colors=(2 2 2 2 2 2)

# Color blocks
block_range=(0 15)
color_blocks="on"
block_width=3
block_height=1

# Text colors
colors=(2 7 2 2 2 7)
```

---

## Phase 4: Advanced Customization

### Switching Between Themes

Create theme-switching scripts in `~/.config/scripts/`:

**`switch-to-interstellar.sh`:**
```bash
#!/bin/bash

# Link Interstellar configs
ln -sf ~/.config/waybar/config ~/.config/waybar/active-config
ln -sf ~/.config/waybar/style.css ~/.config/waybar/active-style.css
ln -sf ~/.config/kitty/kitty.conf ~/.config/kitty/active.conf
ln -sf ~/.config/hypr/interstellar.conf ~/.config/hypr/active-theme.conf

# Restart services
killall waybar
waybar &
swww img ~/Pictures/Wallpapers/Interstellar/main.jpg --transition-type fade

notify-send "Theme Switched" "Interstellar theme activated"
```

**`switch-to-mrrobot.sh`:**
```bash
#!/bin/bash

ln -sf ~/.config/waybar/mrrobot-config ~/.config/waybar/active-config
ln -sf ~/.config/waybar/mrrobot-style.css ~/.config/waybar/active-style.css
ln -sf ~/.config/kitty/mrrobot.conf ~/.config/kitty/active.conf
ln -sf ~/.config/hypr/mrrobot.conf ~/.config/hypr/active-theme.conf

killall waybar
waybar &
swww img ~/Pictures/Wallpapers/MrRobot/main.jpg --transition-type simple

notify-send "Theme Switched" "Mr. Robot theme activated"
```

Make them executable:
```bash
chmod +x ~/.config/scripts/*.sh
```

Add keybindings in `hyprland.conf`:
```conf
bind = $mainMod SHIFT, I, exec, ~/.config/scripts/switch-to-interstellar.sh
bind = $mainMod SHIFT, M, exec, ~/.config/scripts/switch-to-mrrobot.sh
```

---

## Phase 5: Essential Applications

### Firefox Customization

**For Interstellar:**
1. Go to `about:config`
2. Set `toolkit.legacyUserProfileCustomizations.stylesheets` to `true`
3. Find your profile folder in `~/.mozilla/firefox/`
4. Create `chrome/userChrome.css`:

```css
/* Interstellar Firefox Theme */
:root {
    --toolbar-bgcolor: #1a1f29 !important;
    --toolbar-color: #e6e6e6 !important;
    --tab-selected-bgcolor: #ff9933 !important;
}

#navigator-toolbox {
    background-color: #1a1f29 !important;
}

.tabbrowser-tab[selected] {
    background-color: #ff9933 !important;
    color: #0a0e14 !important;
}

#urlbar {
    background-color: #0a0e14 !important;
    color: #e6e6e6 !important;
    border: 1px solid #ff9933 !important;
}
```

### GTK Theme Installation

```bash
paru -S gtk-engine-murrine
```

For Interstellar, use **Nordic-darker** or **Graphite-dark** GTK themes:
```bash
paru -S nordic-theme-git
```

Set in `~/.config/gtk-3.0/settings.ini`:
```ini
[Settings]
gtk-theme-name=Nordic-darker
gtk-icon-theme-name=Papirus-Dark
gtk-font-name=Inter 10
gtk-cursor-theme-name=Bibata-Modern-Classic
```

---

## Phase 6: Final Touches & Optimization

### System Performance

Add to `/etc/sysctl.d/99-swappiness.conf`:
```
vm.swappiness=10
```

### Auto-start Essential Services

In `hyprland.conf`:
```conf
exec-once = dbus-update-activation-environment --systemd WAYLAND_DISPLAY XDG_CURRENT_DESKTOP
exec-once = systemctl --user import-environment WAYLAND_DISPLAY XDG_CURRENT_DESKTOP
exec-once = /usr/lib/polkit-gnome/polkit-gnome-authentication-agent-1
exec-once = swww init
exec-once = waybar
exec-once = dunst
```

### Backup Your Configs

```bash
cd ~
git init --bare $HOME/.dotfiles
alias dotfiles='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
dotfiles config --local status.showUntrackedFiles no
dotfiles add .config/hypr .config/waybar .config/kitty .config/rofi .config/dunst
dotfiles commit -m "Initial Interstellar and Mr. Robot themes"
```

---

## Conclusion

You now have two complete, production-ready themes. The Interstellar theme emphasizes smooth animations, depth, and a sense of vastness. The Mr. Robot theme is raw, terminal-focused, and minimalist. Both are highly functional while being visually distinctive.

Start with Interstellar to get comfortable with Hyprland and the overall workflow. Once you're confident, experiment with Mr. Robot's more aggressive aesthetic. The beauty of this setup is that you can switch between them instantly with a single keybinding.

Remember: rice is a journey, not a destination. You'll continuously tweak and refine these themes as you discover what works best for your workflow. Don't be afraid to experiment and make them your own!