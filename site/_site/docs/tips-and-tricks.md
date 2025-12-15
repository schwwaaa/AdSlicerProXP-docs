## Technical Notes
- Thresholds may require tuning for darker/noisier analog captures.  
- `--merge-gap` prevents fluttering blacks from splitting ad boundaries.  
- `--reencode` ensures exact cuts; stream-copy mode is faster but snaps to keyframes.  
- Logs are stable and reusable for rebuilding cuts or running QC.

## Recommended Usage
- Cleaning archival TV recordings  
- Compiling historical commercial breaks  
- Preparing ad-free show masters  
- Dataset creation for machine learning  
- Automated VHS digitization pipelines

## Quality Tips
- Too many false-positive commercials → raise `--black-min-dur`, raise thresholds, increase `--merge-gap`  
- Missed commercials → lower `--black-min-dur`, adjust thresholds, widen commercial duration window  
- Use `--dry-run` first  
- Use `--reencode` for final/archival exports
- We default to `-c copy` for speed, but for surgical accuracy around non‑keyframes, add `--reencode`.
- Use a slightly higher `--merge-gap` (e.g., 1.0–2.0s) plus small `edge-pad` to stabilize borders.
- If analog noise prevents “full black,” **raise** `--pix-th` and slightly **lower** `--pic-th`.
- We never overwrite; existing outputs trigger suffixes `_1`, `_2`, etc.
- Add `--dry-run` to skip writing media and just produce logs for review.