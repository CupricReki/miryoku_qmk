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

For split + EE_HANDS, build left/right separately, then copy each `.uf2` to the matching half’s drive (RP2040 shows a drive when in bootloader mode; copy the file there, no flasher needed):

```bash
qmk flash -kb crkbd/rev1 -km manna-harbour_miryoku -e CONVERT_TO=splinky_3 -bl uf2-split-left   # build left, then copy to left half
qmk flash -kb crkbd/rev1 -km manna-harbour_miryoku -e CONVERT_TO=splinky_3 -bl uf2-split-right  # build right, then copy to right half
```

## crkbd + Splinky v3

This repo adds a **promicro_to_splinky_3** converter so crkbd (and other Pro Micro boards) can be built for BastardKB Splinky v3 (RP2040). Use `CONVERT_TO=splinky_3` in rules or `-e CONVERT_TO=splinky_3` when compiling.

## Docs

- [Local build with venv](docs/local_build_venv.md)
- [QMK documentation](docs/README.md)
