Perfect — here’s a crisp, copy-paste friendly walkthrough to make **stock Kali** look like your screenshot using your exact palette. I’ll show the **Kitty route (recommended)**, plus **Xfce Terminal** and **GNOME Terminal** variants. Pick one terminal path and follow the shared steps for btop, tmux, etc.

---

# A. Prereqs (all setups)

1. Update and install tools/fonts

```bash
sudo apt update
sudo apt install -y kitty btop tmux neovim fonts-firacode fonts-powerline curl
```

2. Create a working directory

```bash
mkdir -p ~/.config && cd ~/.config
```

Your color palette (for reference):

```
BG  #0B0C16   FG #cddbf4   Cursor #85E1FB
0  #0B0C16  8  #6a6e95
1  #31f759  9  #85ff9d
2  #4fe88f 10  #9cf7c2
3  #50f7d4 11  #a4ffec
4  #829dd4 12  #c4d2ed
5  #86a7df 13  #cddbf4
6  #7cf8f7 14  #d1fffe
7  #85E1FB 15  #ddf7ff
```

---

# B. Terminal choice

## Option 1 — Kitty (recommended)

3. Write Kitty config

```bash
mkdir -p ~/.config/kitty
cat > ~/.config/kitty/kitty.conf <<'EOF'
font_family      FiraCode
bold_font        auto
italic_font      auto
font_size        11.0
cursor_shape     beam
enable_audio_bell no

background #0B0C16
foreground #cddbf4
cursor     #85E1FB
selection_background #1a2134

color0  #0B0C16
color1  #31f759
color2  #4fe88f
color3  #50f7d4
color4  #829dd4
color5  #86a7df
color6  #7cf8f7
color7  #85E1FB
color8  #6a6e95
color9  #85ff9d
color10 #9cf7c2
color11 #a4ffec
color12 #c4d2ed
color13 #cddbf4
color14 #d1fffe
color15 #ddf7ff
EOF
```

4. Launch Kitty and (optionally) set it as default

```bash
kitty &
# In Xfce: Settings > Preferred Applications > Utilities > Terminal Emulator > Kitty
```

---

## Option 2 — Xfce Terminal (Kali default)

3. Create the color scheme file

```bash
mkdir -p ~/.local/share/xfce4/terminal/colorschemes
cat > ~/.local/share/xfce4/terminal/colorschemes/NeonDark.theme <<'EOF'
[Scheme]
Name=NeonDark
ColorForeground=#cddbf4
ColorBackground=#0B0C16
ColorCursor=#85E1FB
ColorPalette=#0B0C16;#31f759;#4fe88f;#50f7d4;#829dd4;#86a7df;#7cf8f7;#85E1FB;#6a6e95;#85ff9d;#9cf7c2;#a4ffec;#c4d2ed;#cddbf4;#d1fffe;#ddf7ff
EOF
```

4. Apply it

* Open **Terminal** → **Edit** → **Preferences** → **Colors** → **Presets** → choose **NeonDark**.
* In **Appearance**, pick **FiraCode** font, size 11–12.

---

## Option 3 — GNOME Terminal

3. Apply palette via dconf (auto-detects your first profile)

```bash
sudo apt install -y dconf-cli
UUID=$(dconf list /org/gnome/terminal/legacy/profiles:/ | head -n1)
BASE="/org/gnome/terminal/legacy/profiles:/$UUID"
dconf write "$BASE/use-theme-colors" false
dconf write "$BASE/background-color" "'#0B0C16'"
dconf write "$BASE/foreground-color" "'#cddbf4'"
dconf write "$BASE/cursor-colors-set" true
dconf write "$BASE/cursor-background-color" "'#85E1FB'"
dconf write "$BASE/cursor-foreground-color" "'#0B0C16'"
dconf write "$BASE/palette" "['#0B0C16','#31f759','#4fe88f','#50f7d4','#829dd4','#86a7df','#7cf8f7','#85E1FB','#6a6e95','#85ff9d','#9cf7c2','#a4ffec','#c4d2ed','#cddbf4','#d1fffe','#ddf7ff']"
```

---

# C. Matching apps and UI

## 5) btop (resource dashboard like the right pane)

```bash
mkdir -p ~/.config/btop/themes
cat > ~/.config/btop/themes/neon-dark.theme <<'EOF'
theme[main_bg]           = "#0B0C16"
theme[main_fg]           = "#cddbf4"
theme[hi_fg]             = "#85E1FB"
theme[title]             = "#c4d2ed"
theme[selected_bg]       = "#1a2134"
theme[selected_fg]       = "#d1fffe"
theme[graph_text]        = "#9cf7c2"
theme[cpu_box]           = "#829dd4"
theme[mem_box]           = "#86a7df"
theme[net_box]           = "#50f7d4"
theme[process_box]       = "#4fe88f"
theme[cpu_start]         = "#31f759"
theme[cpu_mid]           = "#85ff9d"
theme[cpu_end]           = "#d1fffe"
EOF

btop    # Press Options → Theme → neon-dark
```

## 6) tmux status bar (optional but helps the “pane” look)

```bash
cat > ~/.tmux.conf <<'EOF'
set -g mouse on
set -g status-position top
set -g status-bg "#0B0C16"
set -g status-fg "#cddbf4"
set -g message-style "bg=#1a2134,fg=#d1fffe"
set -g pane-border-style "fg=#6a6e95"
set -g pane-active-border-style "fg=#85E1FB"
setw -g window-status-current-format "#[fg=#0B0C16,bg=#85E1FB] #I:#W "
setw -g window-status-format "#[fg=#cddbf4,bg=#0B0C16] #I:#W "
EOF

# Start tmux then reload config if needed
tmux new -s main
# inside tmux:  Ctrl+b  then  :source-file ~/.tmux.conf  Enter
```

## 7) Neovim basics (let it inherit terminal colors)

```bash
mkdir -p ~/.config/nvim
cat > ~/.config/nvim/init.lua <<'EOF'
vim.opt.termguicolors = true
vim.opt.number = true
vim.opt.relativenumber = true
vim.opt.cursorline = true
-- Use terminal palette for highlights (no external theme required)
EOF
```

## 8) Prompt (optional, clean look)

Install Starship prompt:

```bash
curl -sS https://starship.rs/install.sh | sh -s -- -y
mkdir -p ~/.config
cat > ~/.config/starship.toml <<'EOF'
add_newline = false
palette = "neon"
[palettes.neon]
bg = "#0B0C16"
cyan = "#85E1FB"
green = "#31f759"
text = "#cddbf4"
[character]
success_symbol = "[❯](green)"
error_symbol = "[❯](cyan)"
EOF
echo 'eval "$(starship init zsh)"' >> ~/.zshrc
```

Reload your shell:

```bash
exec zsh
```

---

# D. Finishing touches

9. Set a dark neon wallpaper (to match the vibe).

* Xfce: **Settings** → **Desktop** → choose a dark abstract.
* GNOME: **Settings** → **Background**.

10. Truecolor check (just to be sure)

```bash
# You should see smooth gradients (no banding)
awk 'BEGIN{
    s="/\\";
    for (colnum = 0; colnum<77; colnum++) {
        r = 255-(colnum*255/76);
        g = (colnum*510/76); if (g>255) g = 510-g;
        b = (colnum*255/76);
        printf "\033[48;2;%d;%d;%dm", r,g,b;
        printf "\033[38;2;%d;%d;%dm", 255-r,255-g,255-b;
        printf "%s", substr(s,colnum%2+1,1);
    }
    printf "\033[0m\n";
}'
```

---

## Daily use layout (like your screenshot)

* Open **Kitty** (or your terminal)
* Split panes with tmux:

  * Vertical split: `Ctrl+b` then `%`
  * Horizontal split: `Ctrl+b` then `"`
* Run `nvim` on the left pane
* Run `btop` on the right pane

That’s it — you’ll have the same neon, crisp look across the terminal, Neovim, btop, and tmux. If you want me to tweak fonts, line spacing, or export these files as a zip, say the word.
# kali
