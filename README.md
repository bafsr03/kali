No stress — Xfce Terminal sometimes doesn’t pick up custom presets the first time. Do these in order.

# Quick checks

1. Confirm your terminal version (schemes need xfce4-terminal ≥ 1.0):

```bash
xfce4-terminal --version
```

2. Make sure the file is in the **right folder** and named with a **.theme** extension (no “.theme.txt”):

```bash
ls -l ~/.local/share/xfce4/terminal/colorschemes
file ~/.local/share/xfce4/terminal/colorschemes/NeonDark.theme
```

If the `file` output says “text, with CRLF line terminators”, we’ll fix it below.

# Recreate the scheme (user-level, correct syntax)

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

If you previously edited on macOS and the file has Windows line endings, normalize them:

```bash
sudo apt install -y dos2unix
dos2unix ~/.local/share/xfce4/terminal/colorschemes/NeonDark.theme
```

# Reload Xfce Terminal

Close **all** xfce4-terminal windows, then:

```bash
killall xfce4-terminal 2>/dev/null || true
xfce4-terminal &
```

Now open **Edit → Preferences → Colors → Presets**. You should see **NeonDark**.

# If it still doesn’t appear

## Option A — put it system-wide

```bash
sudo mkdir -p /usr/share/xfce4/terminal/colorschemes
sudo tee /usr/share/xfce4/terminal/colorschemes/NeonDark.theme >/dev/null <<'EOF'
[Scheme]
Name=NeonDark
ColorForeground=#cddbf4
ColorBackground=#0B0C16
ColorCursor=#85E1FB
ColorPalette=#0B0C16;#31f759;#4fe88f;#50f7d4;#829dd4;#86a7df;#7cf8f7;#85E1FB;#6a6e95;#85ff9d;#9cf7c2;#a4ffec;#c4d2ed;#cddbf4;#d1fffe;#ddf7ff
EOF
killall xfce4-terminal 2>/dev/null || true
xfce4-terminal &
```

## Option B — force the colors directly in `terminalrc` (works on every build)

This bypasses the Presets menu entirely.

```bash
mkdir -p ~/.config/xfce4/terminal
RC=~/.config/xfce4/terminal/terminalrc

# Create if missing
[ -f "$RC" ] || echo "[Configuration]" > "$RC"

# Remove any old Color* lines
sed -i '/^ColorForeground=/d;/^ColorBackground=/d;/^ColorCursor=/d;/^ColorPalette=/d;/^ColorUseTheme=/d' "$RC"

# Inject our colors
cat >> "$RC" <<'EOF'
ColorUseTheme=FALSE
ColorForeground=#cddbf4
ColorBackground=#0B0C16
ColorCursor=#85E1FB
ColorPalette=#0B0C16;#31f759;#4fe88f;#50f7d4;#829dd4;#86a7df;#7cf8f7;#85E1FB;#6a6e95;#85ff9d;#9cf7c2;#a4ffec;#c4d2ed;#cddbf4;#d1fffe;#ddf7ff
EOF

killall xfce4-terminal 2>/dev/null || true
xfce4-terminal &
```

# Add the slight transparency again (since you’re on Option 2)

Settings Manager → **Window Manager Tweaks → Compositor** → Enable
Terminal → **Preferences → Appearance → Background → Transparent background** → set ~10–15%.

---

If it still doesn’t show, tell me the output of:

```
xfce4-terminal --version
ls -l ~/.local/share/xfce4/terminal/colorschemes
head -n 5 ~/.local/share/xfce4/terminal/colorschemes/NeonDark.theme
```

and I’ll pinpoint it.

