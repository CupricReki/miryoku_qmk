# QMK Firmware (Miryoku)

QMK fork with [Miryoku](https://github.com/manna-harbour/miryoku) keymap, CI builds for selected keyboards, and local build support.

## CI builds

GitLab CI builds firmware on push. Download artifacts from the pipeline (each job → Artifacts).

| Job | Keyboard | Artifacts |
|-----|----------|-----------|
| build-skeletyl-splinky_v2 | BastardKB Skeletyl v2 (Splinky v2 / RP2040) | `*_left.uf2`, `*_right.uf2` |
| build-skeletyl-elitec | BastardKB Skeletyl v2 (Elite-C) | `*.hex` |
| build-crkbd-splinky_v3 | Corne (crkbd) + Splinky v3 (RP2040) | `*_left.uf2`, `*_right.uf2` |

**Split boards (EE_HANDS):** Put each half in bootloader mode (e.g. hold BOOTSEL, plug USB), then **copy** `*_left.uf2` to the left half’s drive and `*_right.uf2` to the right half’s drive. Using the same file on both halves will make both sides report the same handedness.

## Local build

Build locally using a Python venv (no system-wide QMK install). See **[docs/local_build_venv.md](docs/local_build_venv.md)** for setup and usage.

Quick start after venv setup:

```bash
source .venv/bin/activate
qmk compile -kb crkbd/rev1 -km manna-harbour_miryoku -e CONVERT_TO=splinky_3
```

For split + EE_HANDS, use the **build-only** targets so the firmware is built without attempting to flash. Outputs are named `*_left.uf2` and `*_right.uf2` in `.build/`. Put each half in bootloader mode, then copy the matching file to that half’s drive. Use the venv’s make (activate it first or set `QMK_BIN`). For crkbd + Splinky v3, pass `CONVERT_TO` to make:

```bash
make CONVERT_TO=splinky_3 crkbd/rev1:manna-harbour_miryoku:uf2-split-left-build
make CONVERT_TO=splinky_3 crkbd/rev1:manna-harbour_miryoku:uf2-split-right-build
```

Then copy `.build/*_left.uf2` to the left half’s drive and `.build/*_right.uf2` to the right half’s drive.

## crkbd + Splinky v3

This repo adds a **promicro_to_splinky_3** converter so crkbd (and other Pro Micro boards) can be built for BastardKB Splinky v3 (RP2040). Use `CONVERT_TO=splinky_3` in rules or `-e CONVERT_TO=splinky_3` when compiling.

## Docs

- [Local build with venv](docs/local_build_venv.md)
- [QMK documentation](docs/README.md)
