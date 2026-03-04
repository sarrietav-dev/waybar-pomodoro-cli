# Waybar Pomodoro CLI

Minimal Pomodoro timer for Linux that runs in the background and exposes a Waybar indicator.

## Features

- Pure CLI workflow (`start`, `pause`, `resume`, `skip`, `stop`, `status`)
- Background daemon mode with persistent state
- Waybar JSON output (`pomodoro waybar`) with live updates via signal
- Optional desktop notifications with `notify-send`

## Install

```bash
mkdir -p ~/.local/bin
cp pomodoro ~/.local/bin/pomodoro
chmod +x ~/.local/bin/pomodoro
```

Make sure `~/.local/bin` is in your `PATH`.

## Usage

Start with defaults (25/5/15, long break every 4 cycles):

```bash
pomodoro start
```

Start with custom durations:

```bash
pomodoro start --work 50 --break 10 --long-break 20 --long-every 3
```

Other commands:

```bash
pomodoro status
pomodoro pause
pomodoro resume
pomodoro skip
pomodoro stop
pomodoro toggle
```

## Waybar Integration

1. Add `"custom/pomodoro"` to your module list (example in `modules-right`):

```jsonc
"modules-right": [
  "group/tray-expander",
  "bluetooth",
  "network",
  "custom/pomodoro",
  "pulseaudio",
  "cpu",
  "battery"
]
```

2. Add the module definition:

```jsonc
"custom/pomodoro": {
  "exec": "pomodoro waybar",
  "return-type": "json",
  "interval": 1,
  "signal": 11,
  "on-click": "pomodoro toggle",
  "on-click-middle": "pomodoro skip",
  "on-click-right": "pomodoro stop"
}
```

3. Optional styling in `style.css`:

```css
#custom-pomodoro {
  min-width: 12px;
  margin: 0 7.5px;
}

#custom-pomodoro.active {
  color: #7aa364;
}

#custom-pomodoro.paused {
  color: #c18f3f;
}
```

4. Restart Waybar:

```bash
omarchy-restart-waybar
```

## State Files

The daemon stores runtime data in:

- `$XDG_STATE_HOME/pomodoro` (default `~/.local/state/pomodoro`)
- `state` (timer state)
- `pid` (daemon PID)
- `log` (daemon log output)

## Waybar Mouse Actions

- Left click: toggle start/pause/resume
- Middle click: skip phase
- Right click: stop timer
