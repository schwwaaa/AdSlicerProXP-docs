---
layout: default
title: Home
nav_order: 1
---

# How It Works

## 1. Black Segment Detection (ffmpeg blackdetect)
- Uses the ffmpeg `blackdetect` filter to find near-black segments.  
- Segments shorter than `--black-min-dur` are discarded.  
- Segments separated by ≤ `--merge-gap` seconds are merged.

## 2. Commercial Interval Planning
- The **gaps between black segments** are measured.  
- Gaps between `--min-commercial` and `--max-commercial` are classified as commercial blocks.  
- Everything outside these gaps is considered **show content**.  
- Optional pre/post padding extends boundaries for smoother edits.

## 3. Exporting Media
- Individual commercials are exported as separate MP4 files.  
- Remaining keep segments are concatenated into a clean “show” file.  
- `--reencode` ensures perfect frame-accurate cuts when needed.

## 4. Logging & Diagnostics
- `detect.json` — structured black/commercial/keep intervals  
- `detect.csv` — flattened interval table  
- Optional ffmpeg stderr logs

## 5. Batch Processing
- `--input-dir` + `--glob` recursively process entire libraries of recordings.

# Program Template

```bash
adslicerproxp \
  --input INPUT.mp4 \
  --outdir OUTPUT_DIR \
  --black-min-dur VALUE \
  --pix-th VALUE \
  --pic-th VALUE \
  --merge-gap VALUE \
  --edge-pad-pre VALUE \
  --edge-pad-post VALUE \
  --min-commercial VALUE \
  --max-commercial VALUE \
  --include-black \
  --reencode \
  --dry-run \
  -v
```

Batch mode:

```bash
adslicerproxp \
  --input-dir INPUT_FOLDER \
  --glob "*.mp4" \
  --outdir OUTPUT_DIR \
  -v
```

# Arguments

## Input / Output

- **`--input`** *(string, path)*  
  Process a single input video file.

- **`--input-dir`** *(string, path)*  
  Recursively process all matching video files under this directory (used with `--glob`).

- **`--glob`** *(string, glob pattern)*  
  File-matching pattern when using `--input-dir` (e.g. `"*.mp4"`, `"*.mov"`).

- **`--outdir`** *(string, path)*  
  Base output directory where per-input subfolders, show files, commercial clips, and logs are written.

---

## Detection Settings

- **`--black-min-dur`** *(float, seconds)*  
  Minimum duration for a detected black segment. Shorter intervals are treated as noise and ignored.

- **`--pix-th`** *(float, 0.0–1.0)*  
  Pixel luma threshold for ffmpeg `blackdetect`. Lower values = stricter “black” definition.

- **`--pic-th`** *(float, 0.0–1.0)*  
  Fraction of pixels that must be under `pix-th` for a frame to count as black.

- **`--merge-gap`** *(float, seconds)*  
  Merge adjacent black segments separated by less than or equal to this time gap.

---

## Cutting Behavior

- **`--edge-pad-pre`** *(float, seconds)*  
  Extra padding added before detected boundaries (commercial or show) to avoid cutting too tight.

- **`--edge-pad-post`** *(float, seconds)*  
  Extra padding added after detected boundaries for smoother transitions.

- **`--min-commercial`** *(float, seconds)*  
  Minimum gap duration to classify a region as a commercial block.

- **`--max-commercial`** *(float, seconds)*  
  Maximum gap duration to classify a region as a commercial block.

- **`--include-black`** *(boolean flag)*  
  When present, includes surrounding black segments inside exported commercial clips.

- **`--reencode`** *(boolean flag)*  
  When present, re-encodes segments for frame-accurate cuts instead of using stream copy.

- **`--dry-run`** *(boolean flag)*  
  When present, performs detection and writes logs only — no media is cut or written.

---

## Verbosity

- **`-v`** *(boolean flag)*  
  Enable INFO-level logging.

- **`-vv`** *(boolean flag)*  
  Enable DEBUG-level logging (more verbose output, including detailed ffmpeg commands).

# Real World Example

Single-video example:

```bash
adslicerproxp \
  -i vhs_capture.mp4 \
  --outdir ./out \
  --black-min-dur 0.15 \
  --pix-th 0.10 \
  --pic-th 0.97 \
  --merge-gap 2.0 \
  --edge-pad-pre 0.25 \
  --edge-pad-post 0.10 \
  --min-commercial 4.0 \
  --max-commercial 185.0 \
  --include-black \
  --reencode \
  -v
```

Batch example:

```bash
adslicerproxp \
  --input-dir /captures/vhs_rips \
  --glob "*.mp4" \
  --outdir /captures/processed \
  --black-min-dur 0.12 \
  --merge-gap 1.8 \
  --min-commercial 5.0 \
  --max-commercial 210.0 \
  -v
```

## Program Output

```
out/
  <basename>/
    commercials/
      <basename>_ad_0001.mp4
      <basename>_ad_0002.mp4
    show/
      _parts/
        part_0001.mp4
        part_0002.mp4
      <basename>_show.mp4
    logs/
      detect.json
      detect.csv
      ffmpeg_blackdetect.log
```