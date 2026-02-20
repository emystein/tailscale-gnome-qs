# DevContainer Testing Quick Reference

## Quick Start

```bash
# 1. Allow X11 connections (on host)
xhost +local:docker

# 2. Open in VS Code and select DevContainer
# Press F1 → "Dev Containers: Reopen in Container" → Choose GNOME version

# 3. Build and install (inside container)
make build
make install
make enable

# 4. Run GNOME Shell
make run  # For GNOME 43-48

# For GNOME 49 (X11 disabled):
dbus-run-session -- gnome-shell --devkit --wayland
```

## Available GNOME Versions

| GNOME Version | Fedora Version | DevContainer Path |
|---------------|----------------|-------------------|
| GNOME 43      | Fedora 37      | `.devcontainer/gnome-43` |
| GNOME 44      | Fedora 38      | `.devcontainer/gnome-44` |
| GNOME 45      | Fedora 39      | `.devcontainer/gnome-45` |
| GNOME 46      | Fedora 40      | `.devcontainer/gnome-46` |
| GNOME 47      | Fedora 40      | `.devcontainer/gnome-47` |
| GNOME 48      | Fedora 42      | `.devcontainer/gnome-48` |
| GNOME 49      | Fedora 43      | `.devcontainer/gnome-49` |

## Common Commands

### Extension Management

```bash
# List installed extensions
gnome-extensions list

# Show extension info
gnome-extensions info tailscale@joaophi.github.com

# Enable/disable extension
gnome-extensions enable tailscale@joaophi.github.com
gnome-extensions disable tailscale@joaophi.github.com

# Uninstall extension
gnome-extensions uninstall tailscale@joaophi.github.com
```

### Development Workflow

```bash
# Make changes to code...

# Rebuild
make build

# Reinstall
make install

# Restart GNOME Shell (close nested window and run again)
make run
```

### Debugging

```bash
# View GNOME Shell logs
journalctl -f -o cat /usr/bin/gnome-shell

# Check extension errors
journalctl -f | grep -i tailscale

# View extension preferences
gnome-extensions prefs tailscale@joaophi.github.com
```

## Troubleshooting

### Display not working

```bash
# Check DISPLAY variable
echo $DISPLAY

# Set if needed
export DISPLAY=:0
```

### Tailscale socket not accessible

```bash
# On host machine
sudo chmod 666 /var/run/tailscale/tailscaled.sock
```

### Extension not appearing

1. Check if extension is enabled: `gnome-extensions list --enabled`
2. Check logs: `journalctl -f -o cat /usr/bin/gnome-shell`
3. Verify installation: `ls ~/.local/share/gnome-shell/extensions/`
4. Try disabling and re-enabling the extension

### Clean rebuild

```bash
make clean
make build
make uninstall
make install
make enable
```

## Testing Checklist

- [ ] Extension appears in Quick Settings
- [ ] Toggle switches Tailscale on/off
- [ ] Node list displays correctly
- [ ] Exit node selection works
- [ ] Mullvad nodes appear in submenu
- [ ] Settings toggles work (Accept Routes, Shields Up, SSH, DNS)
- [ ] Profile switching works
- [ ] Icons display correctly in system tray
- [ ] No errors in logs

## Cleanup

```bash
# Remove X11 access (on host)
xhost -local:docker

# Clean build artifacts
make clean

# Exit container
# Press F1 → "Dev Containers: Reopen Folder Locally"
```

## Tips

- **Test across versions**: Test on at least GNOME 45, 48, and 49 to ensure compatibility
- **Watch logs**: Keep `journalctl -f` running in a separate terminal to catch errors
- **Incremental testing**: Test each feature individually after changes
- **Use nested mode**: Nested GNOME Shell allows quick iteration without affecting your main session

