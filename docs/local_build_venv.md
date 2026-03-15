# Local build with Python venv

A Python virtual environment at `.venv` is set up for building QMK locally without installing the CLI system-wide.

## One-time setup (already done)

```bash
python3 -m venv .venv
.venv/bin/pip install --upgrade pip
.venv/bin/pip install -r requirements.txt
.venv/bin/pip install qmk
.venv/bin/qmk setup -H "$(pwd)" -y
```

Ensure submodules are initialized (required for build). Use the venv’s `qmk` so `make` can find it:

```bash
# Option A: activate venv first (then qmk is in PATH)
source .venv/bin/activate
make git-submodule

# Option B: override QMK_BIN without activating
make git-submodule QMK_BIN="$(pwd)/.venv/bin/qmk"
```

## Building

Activate the venv so `qmk` and `make` use the same environment, then compile:

```bash
source .venv/bin/activate
qmk compile -kb crkbd/rev1 -km manna-harbour_miryoku -e CONVERT_TO=splinky_3
```

Or without activating, call the venv’s `qmk` directly (from repo root):

```bash
.venv/bin/qmk compile -kb crkbd/rev1 -km manna-harbour_miryoku -e CONVERT_TO=splinky_3
```

Firmware is written under `.build/` and (for some targets) copied to the repo root as `*.hex` or `*.uf2`.

## Optional: dev tools

For pre-commit and other dev checks:

```bash
.venv/bin/pip install -r requirements-dev.txt
```
