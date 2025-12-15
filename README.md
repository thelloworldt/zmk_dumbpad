# Dumbpad ZMK Configuration

This is a ZMK firmware configuration for the Dumbpad macropad with a single rotary encoder, designed for the nice!nano v2 controller.

## Features

- 4x4 key matrix + 3 additional keys (19 keys total)
- Single rotary encoder (top-left position)
- 3 layers with different functions
- Bluetooth and USB support
- Encoder functions change per layer:
  - Layer 0 (Default): Volume up/down
  - Layer 1: Arrow up/down
  - Layer 2: Brightness up/down

## Layout

Default layout matches QMK dumbpad default keymap (numpad):

```
| ENC | 7  | 8  | 9  |
| TAB | 4  | 5  | 6  |
| FN  | 1  | 2  | 3  |
| FN  | 0  | .  | ENT|
| BSP | -  | +  |    |
```

Where:
- ENC = Rotary encoder (push for ESC)
- FN = Hold for function layer
- All number keys are numpad keys (KP_N0-KP_N9)

## Pin Assignments (nice!nano v2)

### Matrix
- **Row pins**: D4, C6, D7, E6, B4 (GPIO 4, 5, 6, 7, 8)
- **Column pins**: F4, F5, F6, F7 (GPIO 21, 20, 19, 18)

### Encoder
- **A pin**: D2 (GPIO 2)
- **B pin**: D3 (GPIO 3)

## Building Locally

1. Install ZMK development environment:
   ```bash
   west init -l config
   west update
   west zephyr-export
   ```

2. Build the firmware:
   ```bash
   west build -s zmk/app -b nice_nano -- -DSHIELD=dumbpad -DZMK_CONFIG="${PWD}/config"
   ```

3. The firmware will be at `build/zephyr/zmk.uf2`

## Building with GitHub Actions

1. Fork this repository
2. Push to main branch or create a pull request
3. GitHub Actions will automatically build the firmware
4. Download the artifact from the Actions tab

## Flashing

1. Put the nice!nano v2 into bootloader mode by double-pressing the reset button
2. A drive called `NICENANO` should appear
3. Copy the `dumbpad_nice_nano.uf2` file to the drive
4. The device will automatically restart with the new firmware

## Customization

### Changing the Keymap
Edit `config/boards/shields/dumbpad/dumbpad.keymap` to modify the key bindings and layers.

### Changing Encoder Resolution
Modify the `steps` and `triggers-per-rotation` values in `dumbpad.overlay`:
- `steps`: Total steps per full rotation (typically 80 for EC11)
- `triggers-per-rotation`: How many triggers to generate per rotation (20 is standard)

### Adding More Layers
Add additional layers in the keymap file following the existing pattern.

## Troubleshooting

- **Encoder not working**: Check the GPIO pins and ensure pull-up resistors are enabled
- **Keys not responding**: Verify the row/column GPIO assignments match your wiring
- **Bluetooth issues**: Ensure `CONFIG_ZMK_BLE=y` is set in `dumbpad.conf`

## Layer Functions

### Layer 0 (Default)
- Standard numpad layout with keypad number keys
- TAB key for navigation
- Encoder: Volume control (rotate), ESC (press)

### Layer 1 (Function/Sub)
- F1-F12 function keys
- System controls (bootloader, reset)
- Media controls (volume, mute)
- Encoder: Page up/down navigation

## License

MIT License - See individual files for copyright information.
