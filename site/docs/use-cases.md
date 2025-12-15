## Use Cases

### Archiving Full Broadcasts With and Without Commercials

**Example:**  
A collector digitizes a 1992 NBC Sunday Night Movie from VHS. They want to preserve the *entire broadcast* — including vintage promos and ads — but they also want a clean, ad-free version for watching.

**Why AdSlicerProXP Is Necessary:**  
Manual cutting risks trimming too tight or leaving partial commercials. AdSlicerProXP automatically detects black slugs that originally separated segments, correctly identifies ad breaks, and exports:

- `/show/vhs_capture_show.mp4` → clean movie  
- `/commercials/vhs_capture_ad_XXXX.mp4` → all commercial blocks  
- `/logs/detect.json` → reproducible archive metadata  

**Outcome:**  
Archivists get *both* the authentic historical object **and** a modernized clean playback master — all generated consistently and without hand-editing.

---

### Digitizing VHS Tapes With Automatic Cleanup

**Example:**  
A preservation group receives 300 home-recorded VHS tapes spanning 1986–2004. Each tape contains 2–6 hours of mixed programming with unpredictable commercial spacing.

**Why AdSlicerProXP Is Necessary:**  
Batch-mode processing (`--input-dir`) allows them to digitize and process entire shelves of tapes at once.  
Threshold tuning ensures blackdetect works on varied analog sources.

**Workflow:**  
```bash
adslicerproxp \
  --input-dir /captures/raw_transfers \
  --glob "*.mp4" \
  --outdir /captures/processed \
  --black-min-dur 0.12 \
  --merge-gap 1.8 \
  -v
```

**Outcome:**  
Hundreds of hours of content are automatically:
- segmented,  
- cleaned,  
- exported into uniform directory structures,  
- and logged for verification.  

This eliminates *months* of manual editing.

---

### Preparing Footage for YouTube or Streaming Uploads

**Example:**  
A creator wants to upload episodes of a 1990 cartoon recorded off Fox Kids.  
The commercial breaks contain copyrighted ads that could trigger Content ID strikes.

**Why AdSlicerProXP Is Necessary:**  
The creator needs predictable, machine-clean removal of commercials without touching the show content.

**Workflow:**  
```bash
adslicerproxp \
  -i cartoon_ep4.mp4 \
  --outdir ./youtube_ready \
  --include-black \
  --reencode \
  -v
```

**Outcome:**  
YouTube receives a clean master with reliable boundaries.  
No leftover half-commercial frames = reduced risk of claims.

---

### Building Training Sets for Machine Learning

**Example:**  
A research lab is training a model to detect “commercial boundary transitions” in old broadcast content.  
They need ground-truth timestamps for:

- black slugs  
- merged black intervals  
- detected ad gaps  
- keep segments  

**Why AdSlicerProXP Is Necessary:**  
It automatically generates the labeled data they need.

**Output Used for ML:**  
- `detect.json` — structured semantic intervals  
- `detect.csv` — tabular timestamp breakdown  
- separate commercial clips — positives  
- show clips — negatives  

**Outcome:**  
A complete labeled dataset is produced *without manual annotation*, enabling ML teams to iterate quickly.

---

### Creating Commercial Compilations

**Example:**  
An editor wants to create a video titled **“All McDonald’s Commercials From 1997 ABC Broadcasts.”**  
They have dozens of full-length recordings and want only the ad blocks, not the shows.

**Why AdSlicerProXP Is Necessary:**  
Commercials are already cleanly extracted into:

```
commercials/
  tape1_ad_0001.mp4
  tape1_ad_0002.mp4
  tape2_ad_0001.mp4
  ...
```

**Workflow:**  
The editor can drop all the extracted clips into a timeline or run a secondary script to sort by brand (e.g., via captioning or logo detection).

**Outcome:**  
An hours-long compilation of ads assembled without touching an editing timeline manually.

---

### 6. High-Volume Cleanup for TV Archives and Libraries

**Example:**  
A university digitization lab processes **1,200** Betacam SP and VHS tapes from 1980–2002.  
Each tape contains long-form recordings with unpredictable ad patterns.

**Why AdSlicerProXP Is Necessary:**  
They require a system that:

- produces *consistent logs* for auditing,  
- preserves commercial breaks,  
- generates clean show files,  
- scales to thousands of hours,  
- and requires minimal manual supervision.

**Workflow:**  
```bash
adslicerproxp \
  --input-dir /library/raw_airchecks \
  --glob "*.mp4" \
  --outdir /library/processed \
  --black-min-dur 0.15 \
  --merge-gap 2.0 \
  --min-commercial 3 \
  --max-commercial 300 \
  -v
```

**Outcome:**  
Every tape now has:
- a clean archival master,  
- a full commercial archive,  
- reproducible logs,  
- uniform directory layout,  
- and metadata ready for ingestion into a digital asset management system.

This brings large institutional archives into a **standardized, searchable** state.

