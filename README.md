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

For split + EE_HANDS, build left/right images, then copy each `.uf2` to the matching half’s drive (put half in bootloader mode first; copy the file to the drive that appears). Use the venv’s make (activate it first or set `QMK_BIN`). For crkbd + Splinky v3, export `CONVERT_TO` so the submake uses the converter. After each make, copy the `.uf2` from `.build/` to that half’s drive (same filename; the binary is left- or right-handed).

```bash
export CONVERT_TO=splinky_3
make crkbd/rev1:manna-harbour_miryoku:uf2-split-left
make crkbd/rev1:manna-harbour_miryoku:uf2-split-right
```

## crkbd + Splinky v3

This repo adds a **promicro_to_splinky_3** converter so crkbd (and other Pro Micro boards) can be built for BastardKB Splinky v3 (RP2040). Use `CONVERT_TO=splinky_3` in rules or `-e CONVERT_TO=splinky_3` when compiling.

## Docs

- [Local build with venv](docs/local_build_venv.md)
- [QMK documentation](docs/README.md)
