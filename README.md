# RODECaster Pro II PipeWire Configuration

PipeWire and WirePlumber configuration for the RODE RODECaster Pro II on Linux.

## Features

- Automatically sets the **Pro Audio** profile exposing all channels
- Renames devices to match Windows naming convention:
  - **USB 1 Chat-Out** - Output to Chat channel (for Discord/Skype/etc.)
  - **USB 1 Main-In** - Input from Main mix output
  - **USB 1 Chat-In** - Input from Chat channel
  - **USB 2 Main-In** - Input/output for secondary device
  - **RODECaster Pro II - Multitrack** - 16-channel input for individual fader recording

## Requirements

- **PipeWire** (audio server)
- **WirePlumber 0.5.x+** (session manager)

Most modern Linux distributions (Fedora 39+, Ubuntu 24.04+, Arch, etc.) ship with compatible versions. Check your version with `wireplumber --version`.

## Installation

### Arch Linux / CachyOS / Manjaro (AUR)

```bash
# With yay
yay -S rodecaster-pro2-pipewire

# With paru
paru -S rodecaster-pro2-pipewire
```

### Manual Installation

```bash
# Clone the repository
git clone https://github.com/Jordan-Milner/rodecaster-pro2-pipewire.git
cd rodecaster-pro2-pipewire

# Build and install (Arch-based)
makepkg -si

# Or copy files manually (any distro)
sudo cp 50-rodecaster-pro2.conf /usr/share/pipewire/pipewire.conf.d/
sudo cp 51-rodecaster-pro2-rename.conf /usr/share/wireplumber/wireplumber.conf.d/
```

### After Installation

Restart PipeWire and WirePlumber:

```bash
systemctl --user restart wireplumber pipewire pipewire-pulse
```

Or simply reconnect your RODECaster Pro II.

## Audio Channels

After installation, your RODECaster Pro II will expose:

| Name | Type | Description |
|------|------|-------------|
| USB 1 Chat-Out | Output | Send audio to the Chat channel (for Discord, etc.) |
| USB 1 Main-In | Output | Input from Main mix (loopback) |
| USB 1 Chat-In | Input | Receive audio from Chat channel |
| USB 2 Main-In | Input/Output | Secondary device Main mix |
| RODECaster Pro II - Multitrack | Input | Receive all 16 individual channels for multitrack recording |

## Troubleshooting

### Device not detected

Check if the device is recognized:

```bash
lsusb | grep -i rode
```

If not showing, try:
1. Reconnect the USB cable
2. Try a different USB port (preferably USB 3.0)
3. Check `journalctl -b | grep -i rode` for errors

### Channels not renamed

Verify WirePlumber loaded the config:

```bash
wpctl status
```

If names are still generic, restart WirePlumber:

```bash
systemctl --user restart wireplumber
```

## License

MIT
