# Pro Micro → Splinky v3 converter

Pin mapping follows the BKB standard (Pro Micro / SparkFun RP2040 compatible footprint). Used for crkbd and other Pro Micro boards with BastardKB Splinky v3.

## Pin map (Pro Micro name → RP2040 GPIO)

Left side: D3=0, D2=1, D1=2, D0=3, D4=4, C6=5, D7=6, E6=7, B4=8, B5=9.
Right side: F4=29, F5=28, F6=27, F7=26, B1=22, B3=20, B2=23, B6=21.
LEDs: D5=17, B0=16.

## crkbd matrix (for wiring checks)

- **Rows:** D4, C6, D7, E6 (GPIO 4, 5, 6, 7)
- **Columns:** F4, F5, F6, F7, B1, B3 (GPIO 29, 28, 27, 26, 22, 20)

Thumb row uses **E6** (GPIO 7). Thumb columns use **F7, B1, B3** (GPIO 26, 22, 20).

- **Left Tab** (L30) = row **E6** + col **F7**
- **Right thumb keys** (R30, R31, R32) = row **E6** + cols **F7, B1, B3**

If thumb keys on one half or Tab on the other don’t work, check wiring and solder for that half’s **E6** (thumb row) and **F7** (first thumb column). The firmware mapping matches the standard pinout; no code change fixes a bad connection.

### Bad GPIO 7 (E6) on the MCU

This converter remaps the thumb row **E6** to **GPIO 8 (B4)** so you can fix a bad GPIO 7 without changing the keyboard PCB:

1. **Firmware:** E6 is defined as 8U in `_pin_defs.h` (B4 pad).
2. **On the Splinky:** Solder a short (bodge wire) from the **E6 pad** (GPIO 7) to the **B4 pad** (GPIO 8). The keyboard’s thumb row is still wired to the E6 pad; the short brings that signal to the good pin.

Rebuild and reflash both halves (same firmware). No PCB changes needed.
